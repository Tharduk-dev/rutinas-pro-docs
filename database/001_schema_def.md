# Esquema de Base de Datos (001_initial_schema)

Este documento describe las 9 tablas iniciales definidas en `infra/supabase/001_initial_schema.sql`. Las convenciones de nomenclatura en base de datos usan `snake_case`, pero en esta Documentación Técnica Extendida (DTE) y a nivel de código de aplicación se mapean a `PascalCase` para las entidades primarias.

## Entidades y Tablas

### 1. Roles (`roles`)
Define los roles dentro de la plataforma.
- `id` (UUID, PK)
- `name` (VARCHAR 255, Unique)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 2. Users (`users`)
Usuarios registrados en el sistema. Relacionados de forma `RESTRICT` con roles.
- `id` (UUID, PK)
- `role_id` (UUID, FK -> Roles)
- `email` (VARCHAR 255, Unique)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 3. Sports (`sports`)
Catálogo de deportes soportados.
- `id` (UUID, PK)
- `name` (VARCHAR 255, Unique)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 4. Exercises (`exercises`)
Ejercicios específicos ligados a un deporte. Relacionados de forma `RESTRICT` con deportes.
- `id` (UUID, PK)
- `sport_id` (UUID, FK -> Sports)
- `name` (VARCHAR 255)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 5. Routines (`routines`)
Rutinas de entrenamiento creadas por usuarios o por defecto. Relacionadas con un deporte específico.
- `id` (UUID, PK)
- `user_id` (UUID, FK -> Users, ON DELETE CASCADE)
- `sport_id` (UUID, FK -> Sports)
- `name` (VARCHAR 255)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 6. RoutineExercises (`routine_exercises`)
Relación de muchos-a-muchos con orden específico entre rutinas y ejercicios.
- `id` (UUID, PK)
- `routine_id` (UUID, FK -> Routines, ON DELETE CASCADE)
- `exercise_id` (UUID, FK -> Exercises)
- `order_index` (INTEGER)
- *Restricción Única:* routine_id, exercise_id
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 7. TrainingSessions (`training_sessions`)
Sesiones de entrenamiento particulares de un usuario, instanciadas a partir de una rutina.
- `id` (UUID, PK)
- `user_id` (UUID, FK -> Users, ON DELETE CASCADE)
- `routine_id` (UUID, FK -> Routines, ON DELETE CASCADE)
- `started_at` (TIMESTAMP)
- `ended_at` (TIMESTAMP, opcional)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 8. SessionExercises (`session_exercises`)
Los ejercicios instanciados que conforman una sesión de entrenamiento, copiados/basados en los de la rutina, permitiendo flexibilidad.
- `id` (UUID, PK)
- `session_id` (UUID, FK -> TrainingSessions, ON DELETE CASCADE)
- `exercise_id` (UUID, FK -> Exercises)
- `order_index` (INTEGER)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### 9. SessionSets (`session_sets`)
Los sets o series reales ejecutados por un usuario en el contexto de un ejercicio de la sesión, almacenando el progreso físico.
- `id` (UUID, PK)
- `session_exercise_id` (UUID, FK -> SessionExercises, ON DELETE CASCADE)
- `set_number` (INTEGER)
- `reps` (INTEGER)
- `weight_kg` (NUMERIC 5,2)
- `completed` (BOOLEAN)
- *Restricción Única:* session_exercise_id, set_number
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

## Índices Configurados
Mejoran las operaciones de join en campos de relación primaria:
- `users.role_id`
- `exercises.sport_id`
- `routines.user_id`, `routines.sport_id`
- `routine_exercises.routine_id`
- `training_sessions.user_id`, `training_sessions.routine_id`
- `session_exercises.session_id`
- `session_sets.session_exercise_id`
