---
evolution:
  - change_id: "EPIC-1-ARCH-BACK"
    epic_reference: "EPIC 1 – Auth & Base Backend"
    date: "2026-02-24"
    description: "Documentación de la arquitectura backend implementada con Fastify."
    affected_modules: ["apps/backend_api"]
---

# Arquitectura del Sistema

## Backend (apps/backend_api)
Se utiliza **Fastify** con una estructura de **Clean Architecture** basada en:
- **Routes**: Definición de paths.
- **Controllers**: Manejo de requests/responses.
- **Services**: Lógica de negocio pura.
- **Repositories**: Acceso a datos (Supabase Client).
