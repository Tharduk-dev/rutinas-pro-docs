---
description: Guía paso a paso para generar un token de acceso a la API de Linear necesario para la Autonomía Agencial.
---

# CONFIGURACIÓN DEL API KEY DE LINEAR

## 1. Objetivo de la Guía
Este manual explica cómo tú (el usuario humano responsable) debes generar un **Personal API Key** en la plataforma de Linear. Este token es indispensable para que el Worker de Integración Agencial de Rutinas Pro pueda automatizar la creación y actualización de issues sin intervención manual.

## 2. Requisitos Previos
- [ ] Tener una cuenta activa en [Linear](https://linear.app).
- [ ] Tener permisos suficientes en el Workspace para generar Personal API Keys.
- [ ] Disponer de un gestor de contraseñas u otra bóveda segura donde almacenar el token temporalmente antes de inyectarlo en Railway.

## 3. Entregable Esperado (Variables)
Al finalizar este manual, deberás tener listos dos valores clave:
```env
LINEAR_API_KEY="lin_api_XYZ..."
LINEAR_TEAM_ID="UUID-del-equipo-en-Linear"
```

## 4. Instrucciones Paso a Paso

### Parte A: Obtener el Personal API Key
1. **Inicia sesión en Linear:** Abre tu navegador y dirígete a [Linear.app](https://linear.app).
2. **Accede a los Ajustes (Settings):** Haz clic en tu avatar o nombre en la esquina superior izquierda y selecciona **Settings** (Ajustes).
3. **Navega a la sección de API:** En el menú lateral izquierdo, bajo la categoría "My Account", haz clic en **API**.
4. **Crea la nueva Key:** En el panel principal, busca la sección "Personal API keys" y haz clic en el botón **New API key**.
5. **Nombra la clave:** Escribe una etiqueta clara para identificar el uso, por ejemplo: `AGENCY_WORKER_RUTINAS_PRO`.
6. **Copia el Token:** Inmediatamente después de crearla, Linear mostrará la clave en pantalla (comienza por `lin_api_`). **Cópiala y guárdala en un lugar seguro al instante**, ya que Linear no la volverá a mostrar.

### Parte B: Obtener el ID del Equipo (Team ID)
1. **Abre un proyecto o issue del equipo objetivo:** Navega por la interfaz de Linear hasta tener visible el tablero de trabajo del equipo (Team) donde quieres que el Agente cree las tareas.
2. **Inspecciona las llamadas de red o la configuración:** La forma más directa de obtener el UUID es a través de las configuraciones del equipo (Settings > Teams > [Nombre de tu Team]), copiando el ID o observando la URL de alguna acción gráfica. (Alternativamente, un desarrollador puede consultarlo mandando una petición GraphQL genérica autenticada a `https://api.linear.app/graphql` consultando sus `teams`).

## 5. Solución de Problemas Frecuentes (FAQ)
- **Problema:** El token generado solo parece tener duración de un mes y luego falla.
  - *Solución:* Linear ofrece a veces tokens de expiración temporal para OAuth. Asegúrate de haber generado estrictamente una **Personal API Key** que no expira por defecto a menos que la revoques.
- **Problema:** He perdido mi token y no lo guardé.
  - *Solución:* Tendrás que volver a la sección Settings > API, revocar el token anterior y crear uno nuevo (repitiendo este manual). Luego deberás actualizar la variable de entorno en tu entorno de despliegue.
