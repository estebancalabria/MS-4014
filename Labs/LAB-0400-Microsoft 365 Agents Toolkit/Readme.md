# LAB-MS4014 — Crear un agente con Microsoft 365 Agents Toolkit

**Curso:** MS-4014 – Build a foundation to build AI agents and extend Microsoft 365 Copilot  
**Duración estimada:** 45 minutos  
**Nivel:** Introductorio  
**Modalidad:** Individual

---

## Objetivo

Al finalizar este laboratorio habrás:

- Instalado la extensión **Microsoft 365 Agents Toolkit** en Visual Studio Code.
- Creado un agente de ejemplo a partir del template **Weather Agent**.
- Ejecutado y probado el agente localmente en el **Microsoft 365 Agents Playground**.

---

## Requisitos previos

| Requisito | Detalle |
|---|---|
| Visual Studio Code | Versión 1.90 o superior |
| Node.js | Versión 20 o superior |
| Cuenta Microsoft 365 | Cuenta de trabajo u organización (no cuenta personal) |
| Azure OpenAI – Endpoint | ✅ Ya disponible (provisto por el instructor) |
| Azure OpenAI – API Key | ✅ Ya disponible (provisto por el instructor) |
| Azure OpenAI – Nombre del modelo | ✅ Ya disponible (provisto por el instructor) |
| Navegador | Microsoft Edge recomendado |

> **Nota:** Para este laboratorio **no es necesario** crear ni configurar ningún recurso de Azure. El endpoint, la API Key y el nombre del modelo ya están disponibles. El instructor los proveerá al inicio de la práctica.

---

## Parte 1 — Instalar la extensión Microsoft 365 Agents Toolkit

### Paso 1.1 — Abrir el panel de extensiones

1. Abre **Visual Studio Code**.
2. Presiona `Ctrl + Shift + X` para abrir el panel de extensiones.

### Paso 1.2 — Buscar e instalar la extensión

1. En el buscador, escribe:

   ```
   Microsoft 365 Agents Toolkit
   ```

2. Selecciona la extensión publicada por **Microsoft**.
3. Haz clic en **Install**.
4. Espera a que finalice la instalación. Verás el ícono de la extensión en la barra lateral izquierda de VS Code.

> **Verificación:** El ícono del Agents Toolkit (un escudo con un agente) debe aparecer en la barra de actividades de VS Code.

---

## Parte 2 — Crear el proyecto del agente

### Paso 2.1 — Abrir el Agents Toolkit

1. En la barra lateral de VS Code, haz clic en el **ícono del Microsoft 365 Agents Toolkit**.
2. Se abre el panel del toolkit con la pantalla de inicio.

### Paso 2.2 — Crear un nuevo agente

1. Haz clic en **Create a New Agent/App**.
2. En la lista de tipos de agente, selecciona:

   ```
   Custom Engine Agent
   ```

### Paso 2.3 — Seleccionar el template

En la pantalla de selección de template, elige:

```
Weather Agent
```

> Este template incluye LangChain y Azure AI Foundry preconfigurados. Es el punto de partida ideal para este laboratorio.

### Paso 2.4 — Seleccionar el servicio LLM

Cuando el toolkit pregunte qué servicio de LLM deseas usar, selecciona:

```
Azure OpenAI
```

### Paso 2.5 — Ingresar los datos del modelo

El toolkit te pedirá tres valores. Ingresa los datos que proveyó el instructor:

| Campo | Valor |
|---|---|
| **Azure OpenAI Key** | *(API Key provista)* |
| **Target URI (Endpoint)** | *(Endpoint provisto)* |
| **Model Name** | *(Nombre del modelo provisto)* |

Ingresa cada valor cuando el asistente lo solicite y presiona `Enter` para confirmar.

### Paso 2.6 — Configurar el proyecto

1. **Lenguaje:** Selecciona `JavaScript` o `TypeScript` según tu preferencia.
2. **Carpeta:** Deja la carpeta por defecto o selecciona una de tu elección.
3. **Nombre de la aplicación:** Escribe un nombre descriptivo, por ejemplo:

   ```
   mi-primer-agente
   ```

4. Presiona `Enter`. El toolkit generará el proyecto automáticamente.

> **Verificación:** VS Code abrirá el proyecto y podrás ver la estructura de archivos en el panel lateral izquierdo, junto con un archivo `README.md` en el panel central.

---

## Parte 3 — Explorar la estructura del proyecto

Antes de ejecutar el agente, dedica unos minutos a revisar los archivos generados.

```
mi-primer-agente/
│
├── src/
│   ├── app.js          # Lógica principal del agente
│   └── index.js        # Punto de entrada
│
├── appPackage/
│   └── manifest.json   # Manifiesto del agente para Microsoft 365
│
├── env/
│   ├── .env.local      # Variables de entorno locales
│   └── .env.playground # Variables para el Agents Playground
│
├── teamsapp.yml        # Configuración del ciclo de vida del agente
└── package.json        # Dependencias del proyecto
```

**Revisa en particular:**

- **`src/app.js`** — Observa cómo se configura LangChain y cómo el agente recibe y responde mensajes.
- **`env/.env.local`** — Confirma que tu API Key y Endpoint están correctamente registrados (el toolkit los inyectó automáticamente).

> **Pregunta de reflexión:** ¿Qué patrón de arquitectura identificás en `app.js`? ¿Cómo se conecta el modelo con la lógica del agente?

---

## Parte 4 — Ejecutar el agente en el Agents Playground

### Paso 4.1 — Iniciar el debug local

1. En el panel del Agents Toolkit, localiza la sección **Run and Debug**.
2. Haz clic en **Debug in Microsoft 365 Agents Playground**.

   > Alternativamente: presiona `F5` asegurándote de que el perfil de debug seleccionado sea *"Debug in Microsoft 365 Agents Playground"*.

3. El toolkit comenzará a preparar el entorno local. Este proceso puede tardar **2 a 4 minutos** la primera vez.

   Durante ese tiempo, VS Code mostrará logs en el panel **Terminal**. Puedes observar cómo se instalan dependencias y se levantan los servicios locales.

### Paso 4.2 — Interactuar con el agente

1. Una vez listo, se abrirá automáticamente una ventana del navegador con el **Microsoft 365 Agents Playground**.
2. En el chat del playground, escribe el siguiente mensaje de prueba:

   ```
   ¿Cuál es el clima en Buenos Aires mañana?
   ```

3. Observa la respuesta del agente. Si el template Weather Agent está correctamente configurado, recibirás una respuesta con información meteorológica generada por el modelo de Azure OpenAI.

> **Verificación:** El agente debe responder con texto coherente. Si ves un error de conexión, verifica que los valores del endpoint y la API Key en `env/.env.local` sean correctos.

### Paso 4.3 — Probar con otras consultas

Realiza al menos **dos pruebas adicionales** con consultas distintas, por ejemplo:

```
What is the weather in Madrid today?
```

```
¿Lloverá en Mendoza esta semana?
```

Observa cómo el agente procesa distintos idiomas y localidades.

---

## Parte 5 — Detener el agente y revisión final

### Paso 5.1 — Detener la sesión de debug

1. Cierra la ventana del navegador del Playground.
2. En VS Code, presiona el botón **Stop** (cuadrado rojo) en la barra de debug, o presiona `Shift + F5`.

### Paso 5.2 — Reflexión final

Responde mentalmente (o por escrito si el instructor lo solicita) las siguientes preguntas:

1. ¿Qué componentes generó automáticamente el toolkit? ¿Cuál sería el esfuerzo de crearlos manualmente?
2. ¿Qué diferencia existe entre ejecutar el agente en el **Agents Playground** versus desplegarlo en **Microsoft 365 Copilot**?
3. ¿En qué escenarios de negocio aplicarías un agente similar al que acabas de crear?

---

## Resumen de lo aprendido

| Tarea | Estado |
|---|---|
| Instalación del Microsoft 365 Agents Toolkit | ✅ |
| Creación de un proyecto con template Weather Agent | ✅ |
| Configuración del modelo Azure OpenAI | ✅ |
| Exploración de la estructura del proyecto | ✅ |
| Ejecución del agente en el Agents Playground | ✅ |
| Prueba de conversación con el agente | ✅ |

---

## Recursos adicionales

- [Documentación oficial: Create JavaScript agents with Agents Toolkit](https://learn.microsoft.com/en-us/microsoft-365/agents-sdk/create-new-toolkit-project-vsc)
- [Instalar el Agents Toolkit](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension)
- [Overview de Microsoft 365 Agents Toolkit](https://learn.microsoft.com/en-us/microsoft-365/developer/overview-m365-agents-toolkit)
- [Explorar el Agents Toolkit en VS Code](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/explore-agents-toolkit)

---

*Laboratorio diseñado para MS-4014 — MCT Esteban Calabria*
