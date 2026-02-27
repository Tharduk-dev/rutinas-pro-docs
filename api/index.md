# API (Backend) - Fastify Gateway

Este documento describe la arquitectura y dependencias incipientes del backend de Rutinas Pro en su estado actual, como parte de la estrategia técnica aprobada.

## Ubicación 
El ecosistema de backend está dividido en dos microservicios principales:
- **Fastify Gateway Principal (Node.js)**: Reside en el entorno monorepo en `apps/backend_api`.
- **Worker Linear Agent (Python)**: Reside en `apps/worker_linear_agent`. Puente de Autonomía Agencial para transcripciones e issues. ([Ver documentación específica del Worker](./worker_linear_agent.md)).

## Stack Tecnológico Principal

El core del sistema es manejado como un API REST estructurado usando **Node.js** y **Fastify**, con validaciones robustas y tipado estricto:

- **Fastify (`fastify`)**: framework Node ligero y altamente eficiente elegido como envoltorio base para la API.
- **Tipado Fuerte y Validación (`zod`)**: se usa Zod para schemas de validación integrados y definición estricta de payloads entrantes/salientes.
- **Autenticación y Seguridad (`@fastify/jwt`)**: protección de endpoints y gestión de identidad mediante web tokens.
- **Documentación Dinámica (`@fastify/swagger`)**: generación de OpenAPI specs y UI interactiva para contratos de endpoint, sincronizado con las rutas.
- **CORS (`@fastify/cors`)**: habilitación de cross-origin restrictions personalizadas para permitir conexión desde los frontends del ecosistema.
- **Supabase SDK (`@supabase/supabase-js`)**: cliente principal de interacción con el ORM / PostgreSQL instanciado en Supabase.
- **Configuración de variables de entorno (`dotenv`)**.

## Scripts de Desarrollo

Conforme al archivo `package.json`, existen los siguientes flujos de ejecución:

- `npm run dev`: Inicia nodemon para recarga en caliente de TypeScript usando `ts-node` sobre `src/app.ts`. Ideal para el loop de desarrollo local.
- `npm run build`: Ejecuta el transpilador TypeScript (`tsc`) emitiendo a la carpeta destino.
- `npm run start`: Ejecuta directamente la salida de transpilación apuntando a `node dist/app.js`.
