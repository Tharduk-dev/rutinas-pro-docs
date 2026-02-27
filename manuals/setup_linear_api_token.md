---
description: Guía de configuración para principiantes de credenciales iniciales de Linear para el Worker Agencial
---

# CONFIGURACIÓN DEL API KEY DE LINEAR Y ENTORNO

## 1. Objetivo de la Guía
Este manual explica cómo tú (el usuario responsable) debes generar un **Personal API Key** en la plataforma de Linear y configurar las credenciales en tu máquina local y en tu entorno de producción (Railway). Estos valores son indispensables para que el Worker Inteligente de Rutinas Pro pueda automatizar la creación y actualización de issues sin intervención manual.

## 2. Requisitos Previos
- [ ] Tener una cuenta activa en [Linear](https://linear.app).
- [ ] Tener acceso al código del proyecto en tu ordenador.
- [ ] Tener acceso al panel de control del proyecto alojado en tu cuenta de [Railway.app](https://railway.app).

## 3. Variables de Entorno (Si aplica)
Al finalizar esta guía, necesitarás asegurar la inyección de estas variables en tu entorno (archivo `.env` o configuraciones de nube):
```env
LINEAR_API_KEY=tu_token_aqui
LINEAR_TEAM_ID=tu_id_de_equipo_aqui
```

## 4. Instrucciones Paso a Paso (Modo Principiante)
> **ATENCIÓN:** Sigue estos pasos visualmente. Si se requiere modificar un archivo local, usa tu editor de código preferido (como Visual Studio Code).

### Paso 1: Generar el Personal API Key de Linear
1. **Abre tu navegador web** y dirígete a [https://linear.app](https://linear.app). Inicia sesión si no lo has hecho.
2. Haz clic en el nombre de tu entorno de trabajo (Workspace) o en tu icono de perfil situado en la esquina superior izquierda.
3. En el menú desplegable, haz clic en **Settings** (Ajustes).
4. En el panel lateral izquierdo, debajo de *My Account* (Mi cuenta), haz clic en **API**.
5. En el centro de la pantalla, localiza el área que dice "Personal API keys" y haz clic en el botón negro que dice **New API key** o el botón `+`.
6. Se abrirá una pequeña ventana pidiendo una etiqueta (Label). Escribe algo descriptivo, por ejemplo: `Rutinas Pro Worker`.
7. Haz clic en **Create** (Crear).
8. **MUY IMPORTANTE:** Inmediatamente aparecerá tu clave (el texto empezará por `lin_api_`). **Cópiala al instante y guárdala en un bloc de notas secreto o en tu gestor de contraseñas**, porque una vez cierres esa ventana de Linear no podrás volver a ver la clave entera nunca más.

### Paso 2: Obtener el Identificador de Equipo (Team ID) de Linear
Ocupamos especificar en qué equipo de Linear el Agente creará las tareas.
1. Haz clic de nuevo en la esquina superior izquierda y ve a **Settings** (Ajustes).
2. En el panel lateral izquierdo, debajo de *Workspace* (Entorno de trabajo), haz clic en **Teams** (Equipos).
3. Haz clic sobre el equipo específico que el agente va a usar (por ejemplo, "Engineering" o el equipo donde gestionas tus issues de Rutinas Pro).
4. Dentro de los ajustes de ese equipo, en la pestaña General básica, busca la sección que dice **Team ID**.
5. Verás una secuencia de letras y números (parecido a `123e4567-e89b-12d3...`). Haz clic en el botón **Copy** que está a su lado para copiar el ID.
6. Guárdalo junto a la API Key que anotaste en el Paso 1.

### Paso 3: Configurar el Archivo `.env` en tu Ordenador (Local)
1. Abre tu carpeta del proyecto (el repositorio de `rutinas-pro`) usando Visual Studio Code o el explorador de archivos de tu ordenador.
2. Navega específicamente a la subcarpeta del worker siguiendo la ruta: `apps/worker_linear_agent`.
3. Crea un nuevo archivo en esa carpeta llamado exactamente `.env` (el punto inicial es obligatorio).
4. Edita el archivo `.env` y pega lo siguiente, reemplazando los textos con los valores reales que guardaste en los pasos anteriores:
   ```env
   LINEAR_API_KEY=tu_token_que_empieza_por_lin_api
   LINEAR_TEAM_ID=el_id_de_tu_equipo
   ```
5. **Comprobación:** Guarda el archivo. Sabrás que ha funcionado si el servidor local que levantaste con Docker (manual anterior) es capaz de comunicarse con Linear.

### Paso 4: Inyectar las variables en Railway (Producción)
1. Dirígete a tu navegador y entra en [Railway](https://railway.app/dashboard).
2. Selecciona tu proyecto de Rutinas Pro.
3. Haz clic en el rectángulo (servicio) que corresponde a tu **Worker de Linear** (el backend de Python que hemos desplegado).
4. En la ventana que se abre a la derecha o en medio, selecciona la pestaña **Variables**.
5. Haz clic en el botón **New Variable** (Nueva variable).
6. En el campo `VARIABLE_NAME` escribe exactamente: `LINEAR_API_KEY` y en `VALUE` pega el token de Linear (del Paso 1). Haz clic en Add.
7. Añade otra variable, llama al nombre `LINEAR_TEAM_ID` y en su valor pega el ID del equipo (del Paso 2). Haz clic en Add.
8. Una vez añadidas, Railway reiniciará el servicio automáticamente de forma segura.

## 5. Solución de Problemas Frecuentes (FAQ)
- **Problema:** En Linear, la API Key que generé ya no se ve cuando refresco la página.
  - *Solución:* Linear oculta permanentemente las claves tras su creación por seguridad. Si la perdiste, debes revocarla (click en el botón de borrar/revoke a su lado), crear una nueva `New API key` y repetir desde el Paso 1.
- **Problema:** El Docker local del Worker sigue buscando la clave aunque ya creé el `.env`.
  - *Solución:* Es posible que el archivo `.env` se haya guardado como `.env.txt`. Asegúrate de que el nombre de archivo es `.env` a secas.
