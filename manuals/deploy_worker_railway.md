---
description: Instrucciones manuales para realizar el despliegue del Worker de Linear Agent en la infraestructura de Railway.
---

# DESPLIEGUE DEL WORKER DE LINEAR EN RAILWAY

## 1. Objetivo de la Guía
Esta guía te explica cómo desplegar el microservicio de integración de Linear (el "Worker Python") en la nube de Railway, vinculando el subdirectorio del repositorio de GitHub e inyectando las variables de entorno sin comprometer la política de seguridad ("Zero Push Policy" de secretos).

## 2. Requisitos Previos
- [ ] Estar autenticado en [Railway.app](https://railway.app).
- [ ] Tener permisos de administrador en el repositorio de GitHub `rutinas-pro`.
- [ ] Haber completado exitosamente la guía de Configuración de Linear (tener a la mano `LINEAR_API_KEY` y `LINEAR_TEAM_ID`).

## 3. Variables de Entorno Necesarias
Deberás configurar manualmente estas variables en el panel de Railway:
```env
LINEAR_API_KEY="lin_api_XYZ..."
LINEAR_TEAM_ID="UUID-del-equipo"
PORT="8000"
```

## 4. Instrucciones Paso a Paso

1. **Crea un nuevo proyecto en Railway:**
   Ve a tu dashboard de Railway y haz clic en **New Project** (o "New" en la esquina superior), y selecciona **Deploy from GitHub repo**.

2. **Selecciona el repositorio de Rutinas Pro:**
   Busca y selecciona el repositorio de github del proyecto (ej. `Tharduk-dev/rutinas-pro`). Si Railway solicita permisos, concédelos para este repositorio.

3. **Inicia el proceso de despliegue y cancélalo temporalmente:**
   Railway intentará desplegar el repositorio completo usando la raíz. Ve rápidamente a la pestaña de **Deployments** del nuevo servicio creado y, si está construyendo, cancélalo haciendo clic derecho o mediante el menú de la build activa. Necesitamos cambiar la configuración primero.

4. **Configura el "Root Directory":**
   - Haz clic en el servicio recién creado dentro del canvas de Railway para abrir sus propiedades.
   - Ve a la pestaña **Settings**.
   - Desplázate hasta la sección de configuración de Build (Build / General).
   - Busca el campo **Root Directory** y escribe exactamente: `/apps/worker_linear_agent`
   - Guarda los cambios. Esto le dice a Railway que busque el `Dockerfile` dentro de esa carpeta de Python específicamente, ignorando el resto del monorepo Node/Flutter.

5. **Inyecta las Variables de Entorno Seguras:**
   - En el mismo panel del servicio, cambia a la pestaña **Variables**.
   - Haz clic en "New Variable" y añade:
     - `LINEAR_API_KEY`: Pega el valor obtenido previamente.
     - `LINEAR_TEAM_ID`: Pega el ID del equipo.
     - `PORT`: `8000` (El Dockerfile expone e inicia en 8000 por defecto).

6. **Desencadena el despliegue final (Manual Deploy):**
   - Una vez aplicadas la nueva ruta raíz y las variables, Railway podría iniciar un despliegue automáticamente.
   - Si no lo hace, pulsa arriba a la derecha el botón **Deploy** o ve a Settings y fuerza un nuevo Trigger.
   - Observa la consola de despliegue (View Logs). Deberías ver cómo Railway detecta el `Dockerfile` basado en `python:3.11-slim`, instala los `requirements.txt` e inicia Uvicorn (`fastapi run src/main.py`).

7. **Verificación:** 
   Ve a la pestaña **Settings**, sección Networking, haz clic en **Generate Domain**.
   Una vez generado, navega a `https://[TU-DOMINIO-RAILWAY]/health`. Si la web devuelve un JSON como `{"status": "ok", "service": "worker_linear_agent"}`, ¡el despliegue ha sido un éxito!

## 5. Solución de Problemas Frecuentes (FAQ)
- **Problema:** En el inicio (logs de Uvicorn) veo un error 500 informando `"LINEAR_API_KEY environment variable not configured"`.
  - *Solución:* Asegúrate de haber escrito el nombre de la variable exactamente en mayúsculas sin espacios en la pestaña Variables de Railway y forzar un redespliegue.
- **Problema:** El servicio falla en arrancar y da "Timeout" o "App crashed", pero el build fue exitoso.
  - *Solución:* Confirma que estableciste la variable de entorno `PORT="8000"`. Railway inyecta de forma dinámica la variable `$PORT` en la nube, pero el `Dockerfile` expone `8000` explícitamente y Fastify (uvicorn) en Python escucha en `8000`; si todo concuerda, la conexión será libre de errores.
