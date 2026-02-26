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

### Desglose Arquitectónico Adicional
- [Documentación y Gobernanza](./documentation_governance.md) - Reglas operativas y convenciones.
- [Arquitectura Backend](./backend_architecture.md) - Flujo y responsabilidades detalladas Fastify.
- [Arquitectura Frontend](./frontend_architecture.md) - Patrones de Clean Architecture con Flutter.
- [Worker Architecture](./worker_architecture.md) - Mecanismos de trabajo en background.
- [Visión del Sistema](./system_overview.md) - Orquestación general.
- [Matriz de Dependencias](./dependency_matrix.md) - Intersecciones entre módulos.
