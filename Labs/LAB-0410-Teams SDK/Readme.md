

# 🧪 LABORATORIO COMPLETO — **Agente de Teams con Teams AI Library (2026)**  
**Objetivo:**  
Crear un agente conversacional para Teams usando `@microsoft/teams-ai`, ejecutarlo localmente y probarlo en Teams.

---

# 1) Crear el proyecto desde cero

## 📁 1.1 Crear carpeta
```bash
mkdir teams-agent
cd teams-agent
npm init -y
```

## 📦 1.2 Instalar dependencias
```bash
npm install @microsoft/teams-ai botbuilder express dotenv
```

---

# 2) Estructura mínima del proyecto

```
/teams-agent
  /src
    index.ts
    agent.ts
  .env
  package.json
  tsconfig.json
```

---

# 3) Configurar TypeScript

`tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "rootDir": "src",
    "outDir": "dist",
    "esModuleInterop": true,
    "strict": false
  }
}
```

---

# 4) Crear el agente

## 🧠 4.1 `src/agent.ts`

```ts
import { Application, TurnState, MemoryStorage } from "@microsoft/teams-ai";
import { OpenAIModel } from "@microsoft/teams-ai/lib/models/openai";

type State = TurnState;

export function createAgent() {
  const model = new OpenAIModel({
    apiKey: process.env.OPENAI_API_KEY!,
    model: "gpt-4o-mini"
  });

  const app = new Application<State>({
    storage: new MemoryStorage(),
    model
  });

  // Mensaje simple
  app.message("/ping", async (context) => {
    await context.sendActivity("Pong desde Teams AI Library.");
  });

  // Acción del agente
  app.ai.action("crearTicket", async (context, state) => {
    const titulo = state.temp.input.titulo;
    await context.sendActivity(`Ticket creado: ${titulo}`);
  });

  // Prompt del agente
  app.ai.importPrompt("./src/prompt.txt");

  return app;
}
```

---

# 5) Crear el prompt del agente

`src/prompt.txt`:

```
Eres un agente de soporte técnico.

Cuando el usuario pida crear un ticket, ejecuta la acción crearTicket con el campo "titulo".
```

---

# 6) Crear servidor Express para exponer el bot

## 🚀 6.1 `src/index.ts`

```ts
import express from "express";
import { createAgent } from "./agent";
import { CloudAdapter, ConfigurationServiceClientCredentialFactory } from "botbuilder";
import dotenv from "dotenv";

dotenv.config();

const app = express();
app.use(express.json());

const agent = createAgent();

const credentialsFactory = new ConfigurationServiceClientCredentialFactory({
  MicrosoftAppId: process.env.BOT_ID!,
  MicrosoftAppPassword: process.env.BOT_PASSWORD!,
  MicrosoftAppType: "MultiTenant"
});

const adapter = new CloudAdapter(credentialsFactory);

app.post("/api/messages", async (req, res) => {
  await adapter.process(req, res, (context) => agent.run(context));
});

app.listen(3978, () => {
  console.log("Bot running on http://localhost:3978/api/messages");
});
```

---

# 7) Variables de entorno

`.env`:

```
OPENAI_API_KEY=tu_api_key
BOT_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
BOT_PASSWORD=tu_password
```

---

# 8) Registrar el bot en Azure AD (obligatorio)

👉 Esto NO se hace en Developer Portal.  
👉 Se hace en **Azure Portal** → “App registrations”.

Pasos:

1. Azure Portal → **App registrations** → New registration  
2. Copiar el **Application (client) ID** → `BOT_ID`  
3. Crear un **Client Secret** → `BOT_PASSWORD`  
4. Agregar permiso:
   - **Bot Framework** → “Bot”  
5. Configurar redirect:
   - `https://token.botframework.com/.auth/web/redirect`

---

# 9) Conectar el bot a Teams

Ahora sí, usás **Developer Portal**, pero SOLO para subir el manifest.

## 9.1 Crear manifest manualmente

`manifest.json`:

```json
{
  "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.16/MicrosoftTeams.schema.json",
  "manifestVersion": "1.16",
  "version": "1.0.0",
  "id": "YOUR-APP-ID",
  "packageName": "com.esteban.agent",
  "name": {
    "short": "Agente Teams AI",
    "full": "Agente con Teams AI Library"
  },
  "description": {
    "short": "Agente conversacional",
    "full": "Agente creado con Teams AI Library"
  },
  "bots": [
    {
      "botId": "BOT_ID",
      "scopes": ["personal"],
      "supportsFiles": false,
      "isNotificationOnly": false
    }
  ],
  "permissions": ["identity", "messageTeamMembers"],
  "validDomains": ["*"]
}
```

Empaquetar:

```
zip app.zip manifest.json
```

---

# 10) Subir a Teams

1. Ir a: `https://dev.teams.microsoft.com/apps` [(dev.teams.microsoft.com in Bing)](https://www.bing.com/search?q="https%3A%2F%2Fdev.teams.microsoft.com%2Fapps")  
2. “Upload custom app”  
3. Seleccionar `app.zip`  
4. Instalar en Teams

---

# 11) Probar el agente en Teams

Abrí Teams → Apps → Tu agente.

Probá:

### ✔️ Comando
```
/ping
```

### ✔️ Acción del agente
```
Necesito crear un ticket urgente para el servidor de base de datos.
```

El agente debe responder:

> Ticket creado: servidor de base de datos

---

# 12) Probarlo desde Copilot (como plugin)

En Copilot (Teams):

```
Usá el agente "Agente Teams AI" para crear un ticket de red.
```

Copilot lo invoca como plugin.

---

# 🧨 RESUMEN DEL LABORATORIO

| Paso | Acción |
|------|--------|
| 1 | Crear proyecto Node |
| 2 | Instalar Teams AI Library |
| 3 | Crear agente |
| 4 | Crear prompt |
| 5 | Crear servidor Express |
| 6 | Registrar bot en Azure AD |
| 7 | Crear manifest |
| 8 | Subir a Teams |
| 9 | Probar en Teams |
| 10 | Probar desde Copilot |

---

# ¿Querés que te genere también?

- README.md listo para tus cursos  
- Diagrama arquitectónico del agente  
- Laboratorio avanzado con herramientas externas

Decime cuál y te lo armo.
