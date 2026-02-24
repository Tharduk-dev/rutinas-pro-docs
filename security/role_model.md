# Modelo de Roles y Permisos (RBAC)

Se definen tres niveles de acceso jerárquicos:

1. **ADMIN**: Control total del sistema, gestión de catálogo global de ejercicios y auditoría de logs.
2. **TRAINER**: Capacidad para crear rutinas, asignarlas a usuarios y ver progreso de sus pupilos.
3. **USER**: Capacidad para ejecutar rutinas, registrar logs propios y editar su perfil personal.

## Implementación
Los roles se almacenan en la tabla `profiles` y se verifican mediante middlewares de autorización en las rutas críticas del backend.
