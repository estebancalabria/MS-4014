# Conectores en Microsoft 365 Copilot y Copilot Studio

> Referencia técnica para arquitectos, trainers y desarrolladores de soluciones Microsoft 365 Copilot.

---

## 1. ¿Qué es un Conector de Copilot?

Un **Copilot Connector** es el mecanismo que permite a Microsoft 365 Copilot y Microsoft Search acceder a datos que viven **fuera del ecosistema Microsoft 365** (SharePoint, Teams, Exchange, etc.).

Sin conectores, Copilot solo puede razonar sobre el contenido del tenant M365. Con conectores, se extiende a Confluence, Jira, Salesforce, ServiceNow, GitHub, bases de datos on-premises, y cualquier fuente personalizada.

```
Fuente externa (Confluence, Jira, Salesforce…)
          ↓
    Copilot Connector
          ↓
    Microsoft Graph Index
          ↓
  M365 Copilot / Microsoft Search
          ↓
Usuario → pregunta en lenguaje natural → Copilot cita la fuente
```

---

## 2. Los dos modelos de Conector

Microsoft 365 Copilot soporta **dos modelos** con arquitecturas distintas:

### 2.1 Synced Connectors (Conectores Sincronizados)

| Atributo | Detalle |
|---|---|
| **Cómo funciona** | Rastrean e indexan contenido externo en Microsoft Graph |
| **Almacenamiento** | Los datos se copian y se indexan semánticamente en Microsoft Graph |
| **Actualización** | Crawl periódico (configurable) |
| **Disponibilidad** | GA — disponible en Commercial, GCC y GCCH |
| **Configuración** | Admin en **Microsoft 365 Admin Center** |
| **Resultado en Copilot** | Copilot puede citar el ítem con su fuente de origen |

**Características clave:**
- Respetan los permisos de la fuente original (ACL)
- Cada ítem incluye: contenido, metadata (título, URL) y lista de control de acceso
- El contenido indexado aparece en Microsoft Search (Office.com, SharePoint, Outlook, Teams, Bing Work)
- Admins pueden crear **search verticals** (pestañas) personalizadas por conector

**¿Cuándo usarlos?**
→ Cuando necesitás que Copilot razone sobre grandes volúmenes de conocimiento externo (wikis, bases de conocimiento, documentación técnica).

---

### 2.2 Federated Connectors (Conectores Federados)

| Atributo | Detalle |
|---|---|
| **Cómo funciona** | Recuperan datos **en tiempo real** vía MCP (Model Context Protocol) |
| **Almacenamiento** | ❌ No indexan datos en Microsoft Graph |
| **Actualización** | Siempre actual — se consulta en el momento de la pregunta |
| **Disponibilidad** | Early Access Preview (Frontier / Targeted Release) |
| **Configuración** | Admin + servidor MCP externo |
| **Resultado en Copilot** | Citas referencian el contenido devuelto en tiempo real desde el servidor MCP |

**¿Cuándo usarlos?**
→ Cuando la frescura del dato es crítica y no tiene sentido indexar (ej: precios en tiempo real, tickets activos, estado de sistemas).

---

### 2.3 Comparativa Synced vs Federated

| | **Synced** | **Federated (MCP)** |
|---|---|---|
| Datos en Microsoft Graph | ✅ Sí | ❌ No |
| Indexación semántica | ✅ Sí | ❌ No |
| Tiempo real | ❌ No (crawl periódico) | ✅ Sí |
| Requiere servidor MCP | ❌ No | ✅ Sí |
| Disponibilidad | GA | Early Access Preview |
| Escenario ideal | Knowledge base, wikis, docs | Datos transaccionales, live data |

---

## 3. Copilot Connectors vs Power Platform Connectors

Es fundamental no confundirlos — son cosas distintas:

| | **Copilot Connectors** | **Power Platform Connectors** |
|---|---|---|
| **Propósito** | Indexar/federar conocimiento externo | Llamar APIs en runtime para leer/escribir/ejecutar |
| **Analogía** | Motor de búsqueda + índice | Puente de API en vivo |
| **Dónde se configura** | M365 Admin Center | Copilot Studio (Tools/Actions) |
| **Resultado** | Grounding con fuentes citadas | El agente ejecuta una acción (crear ticket, enviar email) |
| **Datos en Graph** | ✅ Synced / ❌ Federated | ❌ Nunca |
| **Ejemplos** | Confluence, ServiceNow KB, GitHub | Outlook, Jira (crear issue), SQL Server |

**Regla práctica:**
> Si el objetivo es que Copilot **sepa algo**, usá Copilot Connector.  
> Si el objetivo es que Copilot **haga algo**, usá Power Platform Connector.

---

## 4. Galería de Conectores — Dónde verlos

### 4.1 Copilot Connectors (Graph/Knowledge)

Más de 100 conectores prebuilt organizados por categoría:

| Categoría | Ejemplos |
|---|---|
| Colaboración y comunicaciones | Confluence, MediaWiki |
| CRM | Salesforce, Dynamics 365 |
| Herramientas de desarrollo | GitHub, Azure DevOps, Jira |
| ITSM | ServiceNow, Zendesk |
| Bases de datos | SQL Server, Oracle |
| File systems | Box, Google Drive |
| RRHH | BambooHR, Workday |
| Sitios web | Enterprise Websites (crawler) |

**URLs oficiales:**

```
Galería general (MS + Partners):
https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/connectors-gallery

Conectores de Microsoft:
https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/connectors-gallery-microsoft

Conectores de Partners (certificados para enterprise):
https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/connectors-gallery-partners

Galería en español:
https://learn.microsoft.com/es-es/microsoftsearch/connectors-gallery
```

### 4.2 Power Platform Connectors (Tools/Actions)

Referencia completa de todos los conectores de Power Platform (también disponibles en Copilot Studio):

```
Todos los conectores (referencia completa):
https://learn.microsoft.com/en-us/connectors/connector-reference/

Solo Standard:
https://learn.microsoft.com/en-us/connectors/connector-reference/connector-reference-standard-connectors

Solo Premium:
https://learn.microsoft.com/en-us/connectors/connector-reference/connector-reference-premium-connectors

Solo los publicados por Microsoft:
https://learn.microsoft.com/en-us/connectors/connector-reference/connector-reference-microsoft-connectors
```

---

## 5. Conectores en Copilot Studio — Dónde aparecen

Al crear un agente en Copilot Studio, los conectores aparecen en **dos lugares distintos** según su función:

### 5.1 Como Knowledge Source (Copilot Connectors)

```
Agente → Overview o Knowledge → Add knowledge → Copilot connector
```

- Son los **Graph/Copilot Connectors** indexados en Microsoft Graph
- El admin debe haberlos configurado primero en el **M365 Admin Center**
- Si no aparece el deseado → botón **Advanced** para buscar más
- Requiere licencia M365 Copilot en el mismo tenant para mejores resultados
- Activar **Tenant graph grounding with semantic search** para máxima calidad

### 5.2 Como Tool / Action (Power Platform Connectors)

```
Agente → Tools → Add a tool → Connector → [buscar servicio]
```

- Son los **Power Platform Connectors** (wrappers de API)
- No requieren configuración previa del admin
- Standard: incluidos en todos los planes de Copilot Studio
- Premium: requieren plan específico
- Custom: para APIs propias o no cubiertas

### 5.3 Diagrama en Copilot Studio

```
Copilot Studio — Crear Agente
│
├── Knowledge → Add knowledge → Copilot connector
│   ├── Confluence (si admin lo configuró en M365 Admin Center)
│   ├── ServiceNow KB
│   └── GitHub, Jira, etc.
│
└── Tools → Add a tool → Connector
    ├── Standard: SharePoint, Outlook, Teams, OneDrive
    ├── Premium: Salesforce, Jira (write), SQL Server
    └── Custom: API propia con OpenAPI definition
```

---

## 6. Model Context Protocol (MCP)

### ¿Qué es MCP?

**Model Context Protocol** es un protocolo abierto desarrollado inicialmente por Anthropic (2024) y adoptado rápidamente por Microsoft y otros players del ecosistema de IA. Define un estándar para que los modelos de lenguaje puedan **descubrir y llamar herramientas y fuentes de datos externas** de forma consistente y segura.

> MCP es para los agentes de IA lo que HTTP es para la web: un protocolo estándar de comunicación.

### Arquitectura MCP

```
┌─────────────────────────────────────────────┐
│               MCP Client                    │
│  (Copilot / Agente / IDE / App)             │
└─────────────────┬───────────────────────────┘
                  │  MCP Protocol (JSON-RPC 2.0)
                  │  vía SSE o Streamable HTTP
┌─────────────────▼───────────────────────────┐
│               MCP Server                    │
│  (expone: Tools, Resources, Prompts)        │
└─────────────────┬───────────────────────────┘
                  │
┌─────────────────▼───────────────────────────┐
│           Fuente de datos / API             │
│  (base de datos, servicio externo, archivo) │
└─────────────────────────────────────────────┘
```

### Los tres primitivos de MCP

| Primitivo | Descripción | Analogía |
|---|---|---|
| **Tools** | Funciones que el modelo puede invocar | Endpoints de API |
| **Resources** | Datos o contenido que el servidor expone | Archivos / registros |
| **Prompts** | Templates de prompts reutilizables | Plantillas de instrucciones |

### MCP en el ecosistema Microsoft

Microsoft adoptó MCP como base de los **Federated Connectors** de M365 Copilot y como mecanismo de extensión en Copilot Studio:

| Donde | Cómo se usa MCP |
|---|---|
| **M365 Copilot — Federated Connectors** | El servidor MCP externo responde a queries en tiempo real sin indexar datos |
| **Copilot Studio — MCP Servers** | Los agentes pueden conectarse a MCP servers como herramientas |
| **Power Platform Connectors** | Hay conectores marcados como "MCP Server connector" en la referencia |
| **Azure AI Foundry** | Soporte nativo de MCP para agentes en Azure |
| **GitHub Copilot / VS Code** | Integración MCP para herramientas de desarrollo |

### Flujo de un Federated Connector via MCP

```
Usuario → pregunta a Copilot
          ↓
   Copilot identifica que necesita datos externos
          ↓
   Llama al MCP Server del conector federado
          ↓
   MCP Server consulta la fuente en tiempo real
          ↓
   Devuelve los datos al LLM
          ↓
   Copilot genera respuesta fundamentada
   (cita la fuente de origen, no datos indexados)
```

### ¿Por qué MCP importa para los conectores?

Antes de MCP, cada integración requería código personalizado para cada modelo y cada herramienta — una matriz de N×M integraciones. Con MCP:

- Un servidor MCP expone sus capacidades una sola vez
- Cualquier cliente MCP-compatible puede consumirlo
- Estándar abierto → portabilidad entre modelos (GPT-4, Claude, Gemini, Phi, etc.)

---

## 7. Cómo implementar un Conector

### 7.1 Synced Connector — vía M365 Admin Center

**Quién:** AI Administrator / Global Admin  
**Dónde:** Microsoft 365 Admin Center → Settings → Search & Intelligence → Data sources

Pasos:
1. Ir a M365 Admin Center → **Copilot** → **Connectors** → **Connectors gallery**
2. Elegir un conector prebuilt (ej: Confluence)
3. Configurar la conexión (URL, autenticación)
4. Definir el schema y aplicar **semantic labels** (`title`, `url`, `iconUrl`)
5. Iniciar el crawl — estado: `Crawling → Ready`
6. Habilitar para Copilot y/o Microsoft Search

**Para conectores custom (via API):**
1. Registrar app en Microsoft Entra (Graph API permissions)
2. Implementar la ingesta usando Microsoft Graph Connectors API
3. O usar el **Microsoft 365 Agents Toolkit** / **Graph connector agent**

### 7.2 Power Platform Connector — en Copilot Studio

**Quién:** Maker / Developer  
**Dónde:** Copilot Studio → Agente → Tools

Para conectores prebuilt:
1. Tools → Add a tool → Connector
2. Buscar el servicio (ej: "Jira")
3. Elegir la acción deseada
4. Configurar autenticación (user credentials o service account)

Para conectores custom:
1. Power Apps o Power Automate → New custom connector
2. Definir con OpenAPI (Swagger)
3. Configurar OAuth 2.0 con Microsoft Entra ID
4. Publicar y agregar al agente en Copilot Studio

### 7.3 MCP Server — para agentes en Copilot Studio

1. Implementar un servidor MCP (Node.js, Python, .NET, etc.)
2. Exponer herramientas (`tools`), recursos (`resources`) y/o prompts
3. Publicar el servidor en un endpoint HTTPS accesible
4. En Copilot Studio: **Tools → Add a tool → MCP Server**
5. Ingresar la URL del servidor MCP

---

## 8. Consideraciones de Licensing

| Escenario | Licensing |
|---|---|
| Synced Connectors en M365 Copilot | Requiere **Microsoft 365 Copilot** license en el tenant |
| Power Platform Connectors Standard | Incluidos en todos los planes de Copilot Studio |
| Power Platform Connectors Premium | Requieren **Copilot Studio** plan específico o Power Platform Premium |
| Custom connectors | Incluidos con planes Premium |
| Federated Connectors (MCP) | Early Access Preview — requiere Targeted Release |

---

## 9. Referencias Oficiales

| Recurso | URL |
|---|---|
| Overview Copilot Connectors | https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/overview |
| Galería de conectores | https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/connectors-gallery |
| Conectores Microsoft | https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/connectors-gallery-microsoft |
| Conectores Partners | https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/connectors-gallery-partners |
| Power Platform Connector Reference | https://learn.microsoft.com/en-us/connectors/connector-reference/ |
| Conectores en Copilot Studio (Knowledge) | https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-copilot-connectors |
| Conectores en Copilot Studio (Tools) | https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-connectors |
| Federated Connectors (MCP) | https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/overview |
| Copilot Connectors vs Power Platform | https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-graph-vs-power-platform-connectors |

---

*Documento preparado para training MS-4004 / AI-102 / Copilot Studio.*  
*Última revisión: Mayo 2025.*
