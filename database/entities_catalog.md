# Catálogo de Entidades (Fase 1)

## Base: `users`
- **Descripción**: Autenticación gestionada delegadamente (Supabase Auth).
- **ID**: `UUID`

---

## Tabla: `profiles`
- **Descripción**: Extensión del perfil público del usuario. Relación 1:1 con `auth.users`.
- **Primary Key**: `id` (`UUID`)
- **Foreign Keys**:
  - `id` -> `auth.users(id)` [Constraint: `fk_profiles_users`]
- **Columnas Clave**:
  - `role`: (`VARCHAR`) [USER, TRAINER, ADMIN]
  - `username`: (`VARCHAR`)
- **Constraints**:
  - `unique_profiles_username` (username debe ser único)

---

## Tabla: `workouts`
- **Descripción**: Identificador y metadatos de una rutina creada.
- **Primary Key**: `id` (`UUID`)
- **Foreign Keys**:
  - `trainer_id` (`UUID`) -> `profiles(id)` [Constraint: `fk_workouts_profiles`]
- **Columnas Clave**:
  - `title`: (`VARCHAR`)
  - `description`: (`TEXT`)

---

## Tabla: `exercises`
- **Descripción**: Catálogo base del sistema para los ejercicios.
- **Primary Key**: `id` (`UUID`)
- **Columnas Clave**:
  - `name`: (`VARCHAR`)
  - `target_muscle`: (`VARCHAR`)

---

## Tabla: `workout_exercises`
- **Descripción**: Detalle (N:M) de los ejercicios dentro de una rutina (tabla pivot).
- **Primary Key**: `id` (`UUID`)
- **Foreign Keys**:
  - `workout_id` (`UUID`) -> `workouts(id)` [Constraint: `fk_workout_exercises_workouts`]
  - `exercise_id` (`UUID`) -> `exercises(id)` [Constraint: `fk_workout_exercises_exercises`]
- **Columnas Clave**:
  - `sets`: (`INT`)
  - `reps`: (`INT`)
  - `order_index`: (`INT`)

---

## Tabla: `logs`
- **Descripción**: Histórico de ejecución completado por los usuarios.
- **Primary Key**: `id` (`UUID`)
- **Foreign Keys**:
  - `user_id` (`UUID`) -> `profiles(id)` [Constraint: `fk_logs_profiles`]
  - `workout_exercise_id` (`UUID`) -> `workout_exercises(id)` [Constraint: `fk_logs_workout_exercises`]
- **Columnas Clave**:
  - `actual_reps`: (`INT`)
  - `actual_weight`: (`DECIMAL`)
