# Architecture & Tech Stack de API Backend

**Estado:** Aprobado
**Fecha de Inicialización:** 2026-02-27 (Normalización Inicial)
**Ubicación Principal:** `/apps/backend_api/`

## 1. Overview y Stack Tecnológico
La API del proyecto está desarrollada en Node.js utilizando Fastify. Sirve como el pasarela principal y procesador entre la capa cliente (Flutter) y la base de persistencia de datos (Supabase PostgREST).

**Stack Principal Declarado (Según package.json `0.1.0`):**
- **Framework Core:** `fastify` (^4.26.1)
- **Lenguaje:** TypeScript (`typescript` ^5.3.3)
- **Validación de Esquemas:** `zod` (^3.22.4)
- **Autenticación/Tokens:** `@fastify/jwt` (^8.0.0)
- **Documentación OpenAPI:** `@fastify/swagger` (^8.14.0)
- **Base de Datos Client:** `@supabase/supabase-js` (^2.39.7)
- **Middleware:** `@fastify/cors` (^9.0.1)

## 2. Gobernanza y Clean Architecture
Tal como dictan los estándares en `governance_standards.md`, el backend estructurará obligatoriamente la arquitectura de la siguiente forma por entidad:

- **Controller (`<feature>_controller.ts`):** Gestiona el Request Fastify, valida la entrada mediante la definición de Zod y delega la ejecución al Service. Devuelve HTTP Responses tipados.
- **Service (`<feature>_service.ts`):** Capa donde reside toda lógica de negocio, mapeos de datos transaccionales, e imperativos funcionales. Protege a la base de datos y lanza excepciones en el caso de violaciones semánticas de reglas de entrenamiento.
- **Repository (`<feature>_repository.ts`):** Exclusiva área responsable de interactuar con el SDK de Supabase o bases de datos directas. El controlador NUNCA realiza llamadas SQL.

## 3. Comandos Operativos
- **Iniciar Entorno Local:** `npm run dev` (ejecuta nodemon + var. de entorno con `src/app.ts`).
- **Construcción:** `npm run build` (transpila TypeScript).
- **Arranque en Producción:** `npm run start` (sirve `dist/app.js`).

## 4. Endpoints y Swagger
*(La ruta y el registro de endpoints será actualizado y expandido en `endpoints.md` a medida que la API pase a fases de desarrollo activo de las rutas core).*
Actualmente la API requiere el uso de `@fastify/swagger` para proveer un playground unificado a los devs FrontEnd.
