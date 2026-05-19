# Introducción

## Contexto del módulo

En este módulo, se exploran las opciones de extensibilidad que permiten personalizar o mejorar las capacidades de Copilot. Para ayudar a evaluar cuál es la mejor opción para cada escenario, se utiliza un ejemplo unificado basado en un objetivo empresarial común: mejorar la experiencia de los empleados en torno al soporte técnico de viajes y gastos.

## Escenario

La organización busca simplificar la forma en que los empleados:

- Planifican viajes
- Acceden a la información de políticas
- Administran gastos

El objetivo es:

- Reducir solicitudes de soporte técnico
- Mejorar el cumplimiento de políticas
- Facilitar el acceso a información y asistencia

Esto aplica a actividades como:

- Reserva de viajes
- Consulta de directrices de políticas
- Envío de reembolsos

## Casos de uso que se explorarán

A lo largo del módulo se verá cómo distintas opciones de extensibilidad pueden resolver necesidades específicas como:

- Facilitar la búsqueda de documentos de políticas de viaje
- Guiar a los empleados en el proceso de reserva de viajes
- Ayudar a consultar el estado de reembolsos y enviar gastos
- Soportar la planificación de viajes complejos para asistentes ejecutivos

---

# Opciones de extensibilidad de Microsoft 365 Copilot

Microsoft 365 Copilot puede ampliarse a través de distintos niveles de complejidad, desde la simple indexación de datos hasta la creación de agentes inteligentes totalmente personalizados.

---

# 1. Conectores de Copilot

Permiten indexar contenido externo y hacerlo buscable en Microsoft 365.

- Mejora de búsqueda y recuperación
- Integración sin agentes
- Sin experiencia conversacional avanzada

---

# 2. Agentes declarativos en Copilot Studio

Plataforma low-code para crear agentes conversacionales integrados en Copilot.

- Experiencia guiada
- Integración con Power Platform
- Casos de uso de negocio

---

# 3. Microsoft 365 Agents Toolkit y Visual Studio Code

Herramienta pro-code para crear agentes declarativos avanzados con control total.

- Integración con APIs
- CI/CD
- Componentes avanzados de Teams
- Alto nivel de personalización

---

# 4. Agentes personalizados en Microsoft 365 Copilot

## Descripción general

Los agentes personalizados permiten a las organizaciones ir más allá del modelo declarativo, ofreciendo control total sobre la lógica, orquestación y modelos utilizados.

Pueden construirse con:

- SDK de Teams
- Copilot Studio (modo avanzado)
- SDK de agentes de Microsoft 365

---

## Capacidades clave

- Control total sobre orquestación y razonamiento
- Uso de modelos propios o de terceros (ej. Azure OpenAI)
- Implementación en Microsoft 365 Copilot Chat y Teams
- Soporte para RAG (Retrieval Augmented Generation)
- Agentes autónomos y multiagente

---

## Cuándo usar agentes personalizados

- Se requiere control completo del comportamiento del agente
- Integración con múltiples sistemas externos y APIs
- Uso de LLMs propios o servicios de IA externos
- Experiencias multiplataforma o orientadas a cliente
- Lógica compleja de negocio y orquestación avanzada

---

## Limitaciones

- Mayor complejidad de desarrollo e infraestructura
- Requiere servicios cloud (ej. Azure OpenAI, Azure AI Search)
- No usa directamente el orquestador de Copilot
- Mayor costo y esfuerzo de mantenimiento

---

## Opciones de implementación

### 1. SDK de Teams + Agents Toolkit
- Agentes personalizados en Teams
- Integración con OpenAI/Azure OpenAI
- Publicación en Microsoft 365 Copilot

### 2. Copilot Studio
- Agentes personalizados con lógica extendida
- Publicación en Teams y Microsoft 365

### 3. SDK de Microsoft 365 Agents
- Control total de orquestación y memoria
- Integración con Microsoft Graph y APIs externas
- Implementación multicanal (Copilot, Teams, apps externas)

---

## Escenario aplicado: asistente inteligente de viajes

La empresa necesita un asistente avanzado para viajes complejos.

### Solución

Se construye un agente personalizado que:

- Usa RAG sobre historial de viajes
- Integra Microsoft Graph para calendarios
- Conecta APIs externas de reservas aéreas
- Genera itinerarios dinámicos y personalizados

---

## Análisis del enfoque

Es la mejor opción porque:

- Requiere orquestación avanzada
- Integra múltiples sistemas internos y externos
- Personaliza experiencia por usuario
- Necesita control total del comportamiento del agente

---

## Por qué otras opciones no son adecuadas

- **Conectores de Copilot**: solo recuperación de información
- **Copilot Studio / Agents Toolkit**: insuficiente para orquestación compleja y control total

---

# Arquitectura completa de decisión

1. Conectores → solo datos
2. Copilot Studio → interacción guiada low-code
3. Agents Toolkit → desarrollo pro-code estructurado
4. Agentes personalizados → control total y sistemas avanzados

---

# Cierre del módulo

El resto del módulo evalúa cómo elegir la mejor opción según objetivos, datos y nivel de complejidad.
