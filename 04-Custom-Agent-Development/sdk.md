# Comparación entre SDK de Teams y SDK de Agentes de Microsoft 365**

## **Resumen ejecutivo**
El **SDK de Teams** y el **SDK de Agentes de Microsoft 365** permiten construir experiencias conversacionales, pero **no son equivalentes**.  
La diferencia clave es **dónde vive el agente** y **quién lo orquesta**:

- **SDK de Teams** → El agente vive **fuera de Copilot**, en tu backend.  
- **SDK de Microsoft 365 Agents** → El agente vive **dentro del runtime de Copilot**, con memoria, herramientas y Graph.

---

# **1. Identidad de cada SDK**

## **1.1 SDK de Teams**
- Basado en **Teams AI Library** (`@microsoft/teams-ai`).  
- Permite crear **agentes y bots para Microsoft Teams**.  
- El agente corre en **tu backend** (Node.js).  
- Copilot puede llamarlo como **plugin**, pero **no es nativo**.  
- No tiene memoria persistente ni orquestación avanzada.

## **1.2 SDK de Agentes de Microsoft 365**
- Extensión moderna para crear **agentes nativos de Copilot**.  
- El agente corre **dentro del runtime de Microsoft 365 Copilot**.  
- Tiene **memoria**, **herramientas**, **Graph**, **grounding**, **políticas**, **multicanal**.  
- Es el modelo recomendado para copilots empresariales.

---

# **2. Diferencias estructurales**

| Área | **SDK de Teams** | **SDK de Microsoft 365 Agents** |
|------|------------------|----------------------------------|
| **Dónde vive el agente** | En tu backend | En el runtime de Copilot |
| **Orquestación** | Tu código | Copilot |
| **Memoria** | No | Sí, nativa |
| **Herramientas (tools)** | Limitadas | Nativas, definidas en el agente |
| **Integración con Graph** | Solo vía backend | Nativa, con permisos del agente |
| **Multicanal** | Solo Teams | Teams, Outlook, Word, Excel, Web, apps externas |
| **Publicación** | Teams | Microsoft 365 Copilot |
| **Modelo de ejecución** | Bot Framework + Teams AI Library | Runtime de Copilot |
| **Complejidad** | Baja–media | Alta |
| **Casos ideales** | Bots simples, automatizaciones, integraciones internas | Copilots verticales, agentes empresariales avanzados |

---

# **3. Arquitectura comparada**

## **3.1 SDK de Teams**  
- Tu backend expone `/api/messages`.  
- Teams envía mensajes → tu bot responde.  
- Teams AI Library agrega razonamiento básico.  
- Copilot puede invocarlo como plugin, pero **no lo orquesta**.

```
Teams → Bot Framework → Tu backend → Teams AI Library → Respuesta
```

---

## **3.2 SDK de Microsoft 365 Agents**  
- El agente vive dentro de Copilot.  
- Copilot orquesta: grounding, memoria, herramientas, Graph.  
- Tu backend solo expone APIs opcionales como herramientas.

```
Copilot Runtime → Orquestación → Memoria → Graph → Herramientas → Respuesta
```

---

# **4. Capacidades comparadas**

## **4.1 Razonamiento**
- **Teams SDK:** razonamiento básico vía Teams AI Library.  
- **M365 Agents SDK:** razonamiento avanzado con orquestación nativa.

## **4.2 Memoria**
- **Teams SDK:** no tiene memoria persistente.  
- **M365 Agents SDK:** memoria nativa por usuario y por agente.

## **4.3 Herramientas**
- **Teams SDK:** acciones definidas en código.  
- **M365 Agents SDK:** herramientas declarativas con grounding automático.

## **4.4 Integración con Microsoft Graph**
- **Teams SDK:** solo vía backend.  
- **M365 Agents SDK:** acceso directo con permisos del agente.

## **4.5 Multicanal**
- **Teams SDK:** solo Teams.  
- **M365 Agents SDK:** Teams, Outlook, Word, Excel, Copilot Web, apps externas.

---

# **5. Cuándo usar cada uno**

## **Usar SDK de Teams si:**
- Necesitás un bot o agente **solo para Teams**.  
- Querés integrar sistemas internos rápidamente.  
- No necesitás memoria ni orquestación avanzada.  
- Querés que Copilot lo invoque como **plugin**, no como agente nativo.

## **Usar SDK de Microsoft 365 Agents si:**
- Querés un **agente nativo de Copilot**.  
- Necesitás memoria, herramientas, Graph, grounding.  
- Querés que funcione en **todos los canales**.  
- Estás construyendo un **copilot vertical** para tu empresa.

---

# **6. Ejemplo rápido de código**

## **6.1 SDK de Teams (Teams AI Library)**

```ts
import { Application } from "@microsoft/teams-ai";

const app = new Application({
  model: new OpenAIModel({ apiKey: process.env.OPENAI_API_KEY })
});

app.message("/ping", async (context) => {
  await context.sendActivity("Pong desde Teams.");
});
```

---

## **6.2 SDK de Microsoft 365 Agents**

```ts
import { CopilotAgent } from "@microsoft/m365-agents";

export default new CopilotAgent({
  name: "soporte",
  tools: {
    crearTicket: async ({ titulo }) => {
      return `Ticket creado: ${titulo}`;
    }
  }
});
```

---

# **7. Conclusión**

El **SDK de Teams** y el **SDK de Agentes de Microsoft 365** no compiten:  
sirven para **cosas distintas**.

- **SDK de Teams** → bots y agentes simples **centrados en Teams**.  
- **SDK de Microsoft 365 Agents** → agentes **nativos de Copilot**, con memoria, herramientas, Graph y multicanal.

Si estás construyendo **copilots empresariales**, el camino es el **SDK de Microsoft 365 Agents**.
