# LAB: Primeros pasos en Microsoft Copilot Studio

**Duración estimada:** 45 minutos  
**Nivel:** Introductorio  
**Requisitos:** Tenant de Microsoft 365 con licencia de Copilot Studio (trial o paga)

---

## Objetivos

Al finalizar este laboratorio, el alumno será capaz de:

- Crear un Environment en Power Platform
- Navegar hacia ese environment desde Copilot Studio
- Crear un agente conversacional simple con tópicos y respuestas

---

## PARTE 1 — Crear el Environment

### 1.1 Acceder al Power Platform Admin Center

1. Abrí el navegador y andá a [https://admin.powerplatform.microsoft.com](https://admin.powerplatform.microsoft.com)
2. Iniciá sesión con tu cuenta de Microsoft 365.
3. En el panel izquierdo, hacé clic en **Environments**.

### 1.2 Crear un nuevo Environment

1. Hacé clic en **+ New** en la barra superior.
2. Completá el formulario con los siguientes datos:

   | Campo | Valor sugerido |
   |---|---|
   | Name | `Lab-CopilotStudio` |
   | Region | United States (o tu región) |
   | Type | **Sandbox** |
   | Purpose | Laboratorio de entrenamiento |

3. En la opción **Add a Dataverse data store**, seleccioná **Yes**.
4. Dejá el idioma y la moneda en sus valores por defecto.
5. Hacé clic en **Save**.

> ⏳ El environment tarda entre 1 y 5 minutos en aprovisionarse. El estado cambia de *Provisioning* a *Ready*.

### 1.3 Verificar el Environment

1. Una vez que el estado sea **Ready**, hacé clic sobre el nombre `Lab-CopilotStudio`.
2. Verificá que aparezca la URL del environment (algo como `https://org-xxxxx.crm.dynamics.com`).
3. Anotá esa URL, la vas a necesitar si tenés que acceder directo.

---

## PARTE 2 — Entrar al Environment desde Copilot Studio

### 2.1 Abrir Copilot Studio

1. Andá a [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)
2. Iniciá sesión con la misma cuenta.

### 2.2 Cambiar al Environment correcto

1. En la esquina superior derecha, hacé clic sobre el nombre del environment actual (generalmente dice el nombre de tu tenant por defecto).
2. Se abre un dropdown con la lista de environments disponibles.
3. Buscá y seleccioná **Lab-CopilotStudio**.
4. Copilot Studio se recarga dentro de ese environment.

> ⚠️ Si el environment no aparece en la lista, esperá unos minutos más y recargá la página. El aprovisionamiento puede no haberse completado.

---

## PARTE 3 — Crear un Agente Simple

### 3.1 Crear el agente

1. En el panel izquierdo de Copilot Studio, hacé clic en **Create**.
2. Seleccioná **New agent**.
3. Podés usar el modo conversacional (escribirle al asistente qué hace tu agente) o el modo clásico. Para este lab usá el modo clásico: hacé clic en **Skip to configure**.
4. Completá los datos del agente:

   | Campo | Valor |
   |---|---|
   | Name | `Agente de Soporte Básico` |
   | Description | Agente de prueba para el laboratorio |
   | Instructions | Eres un asistente de soporte. Respondé de forma clara y concisa. |
   | Language | Spanish |

5. Hacé clic en **Create**.

### 3.2 Explorar la interfaz del agente

Una vez creado el agente, vas a ver tres secciones principales:

- **Overview** — configuración general del agente
- **Topics** — los tópicos o flujos conversacionales
- **Actions** — integraciones con conectores y plugins

### 3.3 Editar el tópico de bienvenida

1. Andá a la sección **Topics**.
2. Hacé clic en el tópico llamado **Conversation Start** (o *Inicio de conversación* si está en español).
3. Vas a ver el canvas de edición con nodos de conversación.
4. Hacé clic sobre el nodo de mensaje inicial y editá el texto:

   ```
   ¡Hola! Soy el asistente de soporte del laboratorio.
   ¿En qué puedo ayudarte hoy?
   ```

5. Hacé clic en **Save** (ícono de disquete o botón superior).

### 3.4 Crear un tópico nuevo

1. En la sección **Topics**, hacé clic en **+ Add a topic** → **From blank**.
2. Asignale el nombre: `Consulta de horarios`.
3. En la sección **Trigger phrases**, agregá estas frases de activación (una por vez, presionando Enter):

   - `¿Cuál es el horario de atención?`
   - `horarios`
   - `¿Cuándo atienden?`

4. Debajo del nodo de trigger, hacé clic en **+** para agregar un nodo.
5. Seleccioná **Send a message**.
6. Escribí el siguiente mensaje:

   ```
   Nuestro horario de atención es de lunes a viernes de 9:00 a 18:00 hs (GMT-3).
   Para urgencias fuera de horario, escribí al mail soporte@empresa.com.
   ```

7. Hacé clic en **Save**.

### 3.5 Probar el agente

1. En el panel derecho, hacé clic en **Test your agent** (si no está visible, activalo desde el botón **Test** en la barra superior).
2. En el chat de prueba, escribí:

   ```
   Hola
   ```

   Verificá que responda con el mensaje de bienvenida.

3. Luego escribí:

   ```
   ¿Cuál es el horario de atención?
   ```

   Verificá que el agente responda con el texto del tópico que creaste.

---

## Recursos adicionales

- Documentación oficial: [https://learn.microsoft.com/copilot-studio](https://learn.microsoft.com/copilot-studio)
- Licencias y trial: [https://aka.ms/CopilotStudioTrial](https://aka.ms/CopilotStudioTrial)
- Power Platform Admin Center: [https://admin.powerplatform.microsoft.com](https://admin.powerplatform.microsoft.com)
