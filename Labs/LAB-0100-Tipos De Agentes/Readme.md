# Laboratorio Práctico — Tipos de Agentes de Inteligencia Artificial

## Objetivo

En este laboratorio vamos a explorar distintos tipos de agentes de inteligencia artificial, desde sistemas simples de recuperación de información hasta agentes autónomos capaces de colaborar y perseguir objetivos complejos.

A lo largo de la práctica construiremos y analizaremos tres tipos de agentes:

* Agentes de recuperación
* Agentes de tareas
* Agentes autónomos

---

# En este laboratorio aprenderás

* Qué es un agente basado en RAG
* Cómo funcionan los agentes que utilizan herramientas y conectores
* Qué diferencia existe entre ejecutar tareas y perseguir objetivos
* Qué es un sistema multi-agente
* Cómo funciona conceptualmente [Microsoft AutoGen](https://microsoft.github.io/autogen/stable/?utm_source=chatgpt.com)
* Cómo evolucionan las capacidades de los agentes modernos

---

# Resumen del laboratorio

| Caso   | Tipo de agente         | Tecnología principal  |
| ------ | ---------------------- | --------------------- |
| Caso 1 | Agente de recuperación | RAG + Knowledge       |
| Caso 2 | Agente de tareas       | Tools + Connectors    |
| Caso 3 | Agente autónomo        | Multi-Agent + AutoGen |

---

# Caso 1 — Agente de Recuperación

## Objetivo del caso

Crear un agente que pueda responder preguntas utilizando documentos y conocimiento empresarial.

---

# Paso 1 — Ingresar a Microsoft 365 Copilot

Ingresar a:

[Microsoft 365 Copilot](https://www.microsoft.com/microsoft-365/copilot?utm_source=chatgpt.com)

---

# Paso 2 — Crear un nuevo agente

Crear un nuevo agente desde la opción:

* Create Agent
* New Agent

---

# Paso 3 — Agregar conocimiento

Agregar archivos como:

* PDF
* Word
* PowerPoint

Ejemplos:

* Manual de onboarding
* Procedimientos internos
* Documentación técnica

---

# Paso 4 — Realizar preguntas

Probar preguntas como:

* “¿Cuál es el procedimiento de vacaciones?”
* “¿Qué políticas de seguridad existen?”
* “¿Cómo funciona el proceso de soporte?”

---

# Explicación conceptual

Este tipo de agente utiliza el patrón:

## Retrieval-Augmented Generation (RAG)

El modelo:

* busca información,
* recupera contexto,
* y genera respuestas utilizando conocimiento empresarial.

---

# Idea clave

> El agente solamente responde utilizando conocimiento.
> No ejecuta acciones ni interactúa con sistemas externos.

---

# Caso 2 — Agente de Tareas

## Objetivo del caso

Extender el agente para que pueda utilizar herramientas y ejecutar acciones.

---

# Paso 1 — Abrir el agente creado anteriormente

Ingresar nuevamente al agente creado en el caso anterior.

---

# Paso 2 — Explorar acciones y conectores

Explorar capacidades como:

* Connectors
* Actions
* Integraciones
* Power Automate

---

# Paso 3 — Conectar herramientas

Ejemplos de herramientas:

* Outlook
* Teams
* SharePoint
* APIs
* Power Platform

---

# Paso 4 — Ejecutar tareas

Probar ejemplos como:

* “Enviá un mail al equipo”
* “Buscá información en SharePoint”
* “Creá una tarea”
* “Consultá información externa”

---

# Explicación conceptual

Ahora el agente puede:

* interpretar intenciones,
* elegir herramientas,
* ejecutar acciones,
* y devolver resultados.

---

# Idea clave

> El agente ya no solamente responde preguntas.
> Ahora también puede interactuar con otros sistemas.

---

# Caso 3 — Introducción a Agentes Autónomos con AutoGen

## Objetivo del caso

Comprender cómo funcionan los agentes autónomos y los sistemas multi-agente utilizando [Microsoft AutoGen](https://microsoft.github.io/autogen/stable/?utm_source=chatgpt.com).

---

# Introducción conceptual

En los casos anteriores:

* el usuario pedía información,
* o solicitaba acciones específicas.

Ahora vamos a explorar otra idea:

## Darle un objetivo al agente

Por ejemplo:

> “Organizar el onboarding de un nuevo empleado.”

Para resolverlo, el agente podría:

* consultar documentación,
* enviar correos,
* coordinar reuniones,
* verificar accesos,
* y tomar decisiones durante el proceso.

---

# ¿Qué es AutoGen?

[Microsoft AutoGen](https://microsoft.github.io/autogen/stable/?utm_source=chatgpt.com) es un framework open source de Microsoft para crear sistemas multi-agente.

La idea principal es que múltiples agentes colaboren entre sí para resolver tareas complejas.

---

# Ejemplo conceptual

Un sistema podría tener:

| Agente         | Responsabilidad    |
| -------------- | ------------------ |
| Planner Agent  | Planificar pasos   |
| Research Agent | Buscar información |
| Writer Agent   | Generar contenido  |
| Reviewer Agent | Validar resultados |

---

# Paso 1 — Analizar el flujo autónomo

Observar el siguiente flujo conceptual:

```text id="7h20dc"
Usuario
   ↓
Planner Agent
   ↓
Research Agent
   ↓
Writer Agent
   ↓
Reviewer Agent
   ↓
Resultado Final
```

---

# Explicación

Cada agente:

* tiene un rol,
* colabora con otros agentes,
* y participa en la resolución del objetivo.

---

# Paso 2 — Analizar un ejemplo de código

A continuación veremos un ejemplo simplificado de código.

⚠️ Este código es solamente conceptual y no será ejecutado durante el laboratorio.

```python id="5ltv4x"
from autogen import AssistantAgent

planner = AssistantAgent("planner")
researcher = AssistantAgent("researcher")
reviewer = AssistantAgent("reviewer")

planner.initiate_chat(
    researcher,
    message="Investigar tendencias de IA"
)
```

---

# Explicación del código

En este ejemplo:

* existe un agente planificador,
* otro agente investigador,
* y un agente revisor.

Los agentes pueden comunicarse entre sí para colaborar en una tarea.

---

# Paso 3 — Comprender el cambio de paradigma

## Agente de recuperación

“Respondé usando documentos.”

## Agente de tareas

“Ejecutá esta acción.”

## Agente autónomo

“Cumplí este objetivo.”

---

# Idea clave

> Un agente autónomo puede coordinar múltiples pasos, utilizar herramientas y colaborar con otros agentes para alcanzar un objetivo complejo.

---

# Fin del laboratorio

## En este laboratorio exploramos:

* Agentes de recuperación basados en RAG
* Agentes que utilizan tools y conectores
* Sistemas multi-agente y agentes autónomos con AutoGen

---

# Conclusión

Los agentes modernos pueden entenderse como un espectro de capacidades:

| Nivel        | Capacidad                        |
| ------------ | -------------------------------- |
| Recuperación | Buscar y responder               |
| Tareas       | Ejecutar acciones                |
| Autónomos    | Planificar y coordinar objetivos |

A medida que aumentan las capacidades:

* aumenta la autonomía,
* aumenta la complejidad,
* y también el potencial de automatización dentro de las organizaciones.
