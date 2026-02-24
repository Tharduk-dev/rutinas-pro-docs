---
evolution:
  - change_id: "EPIC-1-API-INIT"
    epic_reference: "EPIC 1 – Auth & Base Backend"
    date: "2026-02-24"
    description: "Definición inicial de endpoints de autenticación y estructura de rutas."
    affected_modules: ["apps/backend_api"]
---

# Catálogo de Endpoints API (v1)

## Auth
- **POST `/v1/auth/login`**: Autenticación de usuario mediante Supabase.
- **GET `/health`**: Estado del servicio.
