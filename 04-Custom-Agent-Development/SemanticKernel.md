# Semantic Kernel
### Curso MS-4014 — Esteban Calabria, MCT

> Semantic Kernel es un SDK open-source de Microsoft para construir agentes de IA y orquestar modelos de lenguaje en aplicaciones empresariales. Soporta **C#**, **Python** y **Java**.  
> **Nota:** Microsoft Agent Framework (MAF) es el sucesor directo de Semantic Kernel, construido por el mismo equipo, listo para producción desde 2025.

---

## ¿Qué es Semantic Kernel?

Semantic Kernel actúa como **middleware de orquestación** entre tu código y los modelos de IA. Permite:

- Conectar LLMs (Azure OpenAI, OpenAI, Hugging Face, Ollama y más)
- Registrar funciones propias como herramientas que el modelo puede invocar
- Gestionar memoria y contexto conversacional
- Construir agentes autónomos y sistemas multi-agente
- Integrar con MCP (Model Context Protocol) y especificaciones OpenAPI

---

## Arquitectura Central

```
┌──────────────────────────────────────────────────┐
│                    Kernel                         │
│  ┌─────────────────┐   ┌────────────────────────┐ │
│  │   AI Services   │   │       Plugins          │ │
│  │  AzureOpenAI    │   │  Semantic + Native     │ │
│  │  OpenAI         │   │  Functions             │ │
│  │  HuggingFace    │   └────────────────────────┘ │
│  │  Ollama / local │   ┌────────────────────────┐ │
│  └─────────────────┘   │  Memory / Vector Store │ │
│                        │  Azure AI Search       │ │
│  ┌─────────────────┐   │  Chroma / Qdrant       │ │
│  │  Function Call  │   └────────────────────────┘ │
│  │  Auto / Manual  │                               │
│  └─────────────────┘                               │
└──────────────────────────────────────────────────┘
```

El **Kernel** es un contenedor de inyección de dependencias (similar a Spring IoC o .NET DI). Todos los servicios, plugins y configuraciones se registran en él y están disponibles globalmente durante la ejecución.

---

## Instalación

**Python**
```bash
pip install semantic-kernel
```

**C#**
```bash
dotnet add package Microsoft.SemanticKernel
```

**Java**
```xml
<dependency>
  <groupId>com.microsoft.semantic-kernel</groupId>
  <artifactId>semantickernel-api</artifactId>
</dependency>
```

---

## Quickstart

### Python

```python
import asyncio
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion
from semantic_kernel.connectors.ai.open_ai import OpenAIChatPromptExecutionSettings

kernel = Kernel()

kernel.add_service(
    AzureChatCompletion(
        deployment_name="gpt-4o",
        endpoint="https://<tu-recurso>.openai.azure.com/",
        api_key="<tu-api-key>"
    )
)

result = await kernel.invoke_prompt("Explicá qué es un agente de IA en dos oraciones.")
print(result)
```

### C#

```csharp
using Microsoft.SemanticKernel;

var kernel = Kernel.CreateBuilder()
    .AddAzureOpenAIChatCompletion(
        deploymentName: "gpt-4o",
        endpoint: "https://<tu-recurso>.openai.azure.com/",
        apiKey: "<tu-api-key>")
    .Build();

var result = await kernel.InvokePromptAsync("Explicá qué es un agente de IA en dos oraciones.");
Console.WriteLine(result);
```

---

## Plugins

Los plugins son la forma en que Semantic Kernel expone funciones al modelo. Hay dos tipos:

### Funciones Semánticas (prompts)

```python
from semantic_kernel.functions import KernelFunctionFromPrompt

summarize = KernelFunctionFromPrompt(
    function_name="summarize",
    prompt="Resumí el siguiente texto en un párrafo: {{$input}}",
)

kernel.add_function(plugin_name="TextTools", function=summarize)
result = await kernel.invoke(summarize, input="Semantic Kernel es un SDK de orquestación...")
```

### Funciones Nativas (código Python/C#)

```python
from semantic_kernel.functions import kernel_function

class WeatherPlugin:
    @kernel_function(description="Obtiene el clima actual de una ciudad")
    def get_weather(self, city: str) -> str:
        # lógica real aquí
        return f"El clima en {city} es soleado, 22°C"

kernel.add_plugin(WeatherPlugin(), plugin_name="Weather")
```

```csharp
public class WeatherPlugin
{
    [KernelFunction, Description("Obtiene el clima actual de una ciudad")]
    public string GetWeather(string city) => $"El clima en {city} es soleado, 22°C";
}

kernel.Plugins.AddFromType<WeatherPlugin>("Weather");
```

---

## Function Calling

Con `FunctionChoiceBehavior.Auto()`, el modelo decide automáticamente qué funciones invocar según el mensaje del usuario. Semantic Kernel actúa como middleware: convierte la solicitud del modelo en una llamada real a la función y devuelve el resultado.

```csharp
OpenAIPromptExecutionSettings settings = new()
{
    FunctionChoiceBehavior = FunctionChoiceBehavior.Auto()
};

var result = await kernel.InvokePromptAsync(
    "¿Qué temperatura hace en Buenos Aires?",
    new KernelArguments(settings)
);
```

Flujo interno:
```
Usuario → Kernel → LLM (decide llamar WeatherPlugin.GetWeather)
       → Kernel ejecuta la función → resultado devuelto al LLM
       → LLM genera respuesta final → Usuario
```

---

## Memory y RAG

Semantic Kernel incluye soporte para búsqueda semántica sobre vectores, habilitando patrones RAG (Retrieval-Augmented Generation).

```python
from semantic_kernel.connectors.memory.azure_ai_search import AzureAISearchCollection
from semantic_kernel.data import VectorStoreRecordCollection

# Configurar vector store
collection = AzureAISearchCollection(
    collection_name="documentos",
    data_model_type=MiModelo
)

# Buscar por similitud semántica
results = await collection.search("políticas de vacaciones", top=3)
```

**Vector stores soportados:**
- Azure AI Search
- Chroma
- Qdrant
- Elasticsearch
- Pinecone
- Redis
- In-memory (para desarrollo/tests)

---

## Agentes

### ChatCompletionAgent

El tipo de agente más común. Combina instrucciones del sistema, historial de conversación y acceso a plugins.

```python
from semantic_kernel.agents import ChatCompletionAgent
from semantic_kernel.contents import ChatHistory

agent = ChatCompletionAgent(
    kernel=kernel,
    name="AsistenteRRHH",
    instructions="Sos un asistente de Recursos Humanos. Respondés preguntas sobre políticas internas.",
)

history = ChatHistory()
history.add_user_message("¿Cuántos días de vacaciones tengo?")

async for response in agent.invoke(history):
    print(response.content)
```

```csharp
using Microsoft.SemanticKernel.Agents;

var agent = new ChatCompletionAgent()
{
    Name = "AsistenteRRHH",
    Instructions = "Sos un asistente de RRHH. Respondés preguntas sobre políticas internas.",
    Kernel = kernel
};

await foreach (var response in agent.InvokeAsync("¿Cuántos días de vacaciones tengo?"))
{
    Console.WriteLine(response.Message);
}
```

### Patrones Multi-Agente

```python
from semantic_kernel.agents import AgentGroupChat, ChatCompletionAgent
from semantic_kernel.agents.strategies import TerminationStrategy

investigador = ChatCompletionAgent(kernel=kernel, name="Investigador",
    instructions="Investigás temas y presentás hechos concretos.")

redactor = ChatCompletionAgent(kernel=kernel, name="Redactor",
    instructions="Tomás los hechos del Investigador y escribís un resumen ejecutivo.")

chat = AgentGroupChat(agents=[investigador, redactor])

async for response in chat.invoke("Investigá el impacto de los agentes de IA en empresas B2B"):
    print(f"[{response.name}]: {response.content}")
```

**Patrones de orquestación disponibles:**
| Patrón | Descripción |
|---|---|
| Sequential | Los agentes se ejecutan en orden definido |
| Concurrent | Los agentes trabajan en paralelo |
| Handoff | Un agente transfiere el control a otro según contexto |
| Group Chat | Múltiples agentes colaboran en una conversación |
| Magentic | Orquestación dinámica basada en el meta-agente |

---

## Integración con MCP

Semantic Kernel soporta **Model Context Protocol (MCP)**, permitiendo exponer el kernel como servidor MCP o consumir herramientas externas vía MCP.

```python
# Exponer el Kernel como servidor MCP
server = kernel.as_mcp_server(server_name="mi_agente_sk")
```

---

## Observabilidad

Semantic Kernel incluye soporte nativo para telemetría con **OpenTelemetry**, compatible con Azure Monitor y Application Insights.

```csharp
builder.Services.AddLogging(c => c.AddConsole().SetMinimumLevel(LogLevel.Trace));

// Filtros para interceptar llamadas (pre/post ejecución)
kernel.FunctionInvocationFilters.Add(new MiFiltroDeAuditoria());
```

---

## Microsoft Agent Framework (MAF)

> **Microsoft Agent Framework es el sucesor directo de Semantic Kernel.**  
> Construido por el mismo equipo, combina Semantic Kernel + AutoGen en un único orquestador enterprise.

| | Semantic Kernel | Microsoft Agent Framework |
|---|---|---|
| Estado | Estable (v1.x) | Production-ready (v1.0+) |
| Multi-agente | Agent Framework (dentro de SK) | Nativo, primera clase |
| AutoGen | No incluido | Integrado |
| Workflows | Process Framework | Graph-based workflows |
| Recomendación | Proyectos existentes | Proyectos nuevos |

La migración desde SK a MAF es incremental — el código de SK 1.x es compatible.

```csharp
// Semantic Kernel (existente)
using Microsoft.SemanticKernel;

// Microsoft Agent Framework (nuevo)
using Microsoft.AgentFramework;  // mismo equipo, nueva superficie
```

---

## Recursos

| Recurso | URL |
|---|---|
| Documentación oficial SK | https://learn.microsoft.com/semantic-kernel |
| Repo GitHub | https://github.com/microsoft/semantic-kernel |
| Notebooks Python (inicio rápido) | https://github.com/microsoft/semantic-kernel/tree/main/python/notebooks |
| Microsoft Agent Framework | https://learn.microsoft.com/agent-framework |
| AI Agents for Beginners | https://github.com/microsoft/ai-agents-for-beginners |
| Discord SK | https://aka.ms/SKDiscord |

---

*Materiales desarrollados por Esteban Calabria — MCT | Azure & AI Consultant*  
*Última actualización: Mayo 2026*
