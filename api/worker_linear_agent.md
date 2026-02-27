# Worker Linear Agent (API Bridge)

Este documento describe la arquitectura técnica, las variables de entorno necesarias y los contratos de endpoint del microservicio en Python para la integración de la **Autonomía Agencial** con **Linear** (EPIC 2.1).

## Ubicación y Arquitectura
El código se ubica en el monorepo bajo `apps/worker_linear_agent`. 
Se trata de un servicio HTTP stateless, rápido y minimalista construido sobre:
- **Python 3.11+**
- **FastAPI** / **Uvicorn**
- **Pydantic** para los modelos DTO.
- **Requests** para la comunicación GraphQL síncrona con el backend de Linear.

Es un microservicio "puente" completamente independiente de la API de Fastify (Node.js), orquestado a través de contenedores Docker (`Dockerfile` provisto en su directorio).

## Configuración y Variables de Entorno
El Worker no hardcodea secretos y confía plenamente en la inyección de estas variables:

- `LINEAR_API_KEY` *(Obligatorio)*: El token de autorización ("Personal API Key" o "OAuth Token") para invocar la API GraphQL de Linear. Se envía en los headers como `Authorization: <LINEAR_API_KEY>`.
- `LINEAR_TEAM_ID` *(Obligatorio)*: UUID del equipo de Linear donde se van a crear los Issues automáticamente.
- `PORT` *(Opcional)*: Puerto donde levantará Uvicorn, por defecto configurado a `8000`.

## Endpoints REST

La API expone prefijos bajo `/linear`. 

### `GET /linear/issues`
Recupera el roadmap activo (lista de issues) desde Linear para el equipo configurado en el entorno (`LINEAR_TEAM_ID`).

- **Headers**: 
  - (Manejado internamente por el Worker)
- **Request**:
  - GET (Sin Body)
- **Response Success (200 OK)**:
  ```json
  [
    {
      "id": "UUID-del-issue",
      "identifier": "ENG-1",
      "title": "Añadir soporte para webhooks",
      "description": "Detalles del issue...",
      "state": {
        "name": "Todo"
      },
      "url": "https://linear.app/..."
    }
  ]
  ```

### `POST /linear/issue`
Crea un Issue nuevo en el proyecto apuntado por la variable de entorno `LINEAR_TEAM_ID`.

- **Headers**: 
  - `Content-Type: application/json`
- **Body Request (`application/json`)**:
  ```json
  {
    "title": "string",
    "description": "string",
    "status": "string | null" // (Opcional) Ej. "Todo"
  }
  ```
- **Response Success (201 Created)**:
  ```json
  {
    "id": "UUID-del-issue",
    "identifier": "RTN-12",
    "url": "https://linear.app/..."
  }
  ```

### `PATCH /linear/issue/{id}`
Actualiza el estado ("State" / workflow state) de un issue existente previamente en Linear. Tras bambalinas invoca consultas GraphQL dinámicas para emparejar el string `status` insertado con los estados válidos del workflow del Team.

- **Headers**: 
  - `Content-Type: application/json`
- **Path Parameter**:
  - `id`: El UUID real del issue (`id` retornado en la creación, NO el identificador corto).
- **Body Request (`application/json`)**:
  ```json
  {
    "status": "string" // Ej. "Todo", "In Progress", "Ready for Review"
  }
  ```
- **Response Success (200 OK)**:
  ```json
  {
    "success": true,
    "message": "Issue status updated to In Progress"
  }
  ```

> **Alerta Operativa**: Asegurarse de tener aprovisionado el entorno en Railway o local con una `LINEAR_API_KEY` real antes de invocar cualquier endpoint bajo `/linear/`, de lo contrario retornará HTTP 500 por seguridad.
