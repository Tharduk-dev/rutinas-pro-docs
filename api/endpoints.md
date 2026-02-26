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

### **POST `/v1/auth/login`**
- **Descripción**: Autenticación de usuario generando JWT a través de Supabase.
- **Auth required**: No
- **Roles allowed**: Public
- **Request schema**:
  ```json
  {
    "email": "user@example.com",
    "password": "secure_password"
  }
  ```
- **Response schema**:
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI...",
    "user": {
      "id": "uuid-string",
      "email": "user@example.com"
    }
  }
  ```
- **Error codes**:
  - `400 Bad Request`: Formato de credenciales inválido.
  - `401 Unauthorized`: Credenciales incorrectas.
  - `500 Internal Server Error`: Problemas de conexión con Auth Provider.

---

### **GET `/health`**
- **Descripción**: Validación del estado de conectividad de los servicios.
- **Auth required**: No
- **Roles allowed**: Public
- **Response**: `200 OK`
