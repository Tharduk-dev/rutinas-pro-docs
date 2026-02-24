# Descripción del Esquema de Base de Datos

La base de datos PostgreSQL en Supabase gestiona la persistencia de la Fase 1 (Core Training).

## Entidades Principales
- **users**: Gestionada por Supabase Auth.
- **profiles**: Extensión de datos de usuario (nombre, biografía, nivel).
- **workouts**: Cabecera de las rutinas creadas por entrenadores o usuarios.
- **exercises**: Catálogo de ejercicios disponibles en el sistema.
- **workout_exercises**: Tabla pivot que define la composición de una rutina (series, repeticiones, orden).
- **logs**: Registro histórico de ejecuciones de ejercicios por parte del usuario.

## Convenciones
- Nombres de tabla en `snake_case` plural.
- IDs mediante `UUID` generados en servidor.
- Timestamps `created_at` y `updated_at` obligatorios en cada tabla.
