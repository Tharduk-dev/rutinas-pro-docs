# Entidades y Esquema de Base de Datos (Supabase / PostgreSQL)

**Estado:** Aprobado
**Fecha de Inicialización:** 2026-02-27 (Normalización Inicial)
**Versión de Esquema Actual:** `001_initial_schema.sql`

Este documento describe la estructura base de datos de "Controla tus Rutinas". El esquema relacional está diseñado para soportar multi-roles, la creación dinámica de rutinas deportivas y el registro avanzado de sesiones de entrenamiento.

## 1. Contexto Global
El ecosistema completo gira alrededor de tres pilares principales:
- **Gestión de Usuarios (Auth/Roles):** Determina si un perfil es administrador, entrenador o usuario estándar.
- **Catálogo Deportivo:** Define `sports` y sus `exercises` asociados.
- **Motor de Entrenamiento:** Permite crear plantillas (`routines`), asignarle ejercicios ordenados (`routine_exercises`) y registrar instancias reales de entrenamiento (`training_sessions`) con sets detallados (`session_sets`).

## 2. Definición de Tablas Principales

### 2.1. `roles`
Define las capacidades operativas del individuo en la plataforma.
- `id` (UUID, PK)
- `name` (VARCHAR 255, UNIQUE): Nombre del rol ("user", "trainer", "admin").

### 2.2. `users`
Contiene la información de perfil asociada a la capa de autenticación externa de Supabase Auth.
- `id` (UUID, PK)
- `role_id` (UUID, FK -> `roles.id` RESTRICT)
- `email` (VARCHAR 255, UNIQUE)

### 2.3. `sports`
Catálogo maestro de disciplinas deportivas soportadas por la app.
- `id` (UUID, PK)
- `name` (VARCHAR 255, UNIQUE): Nombre del deporte (e.g. "Gimnasio", "Calistenia").

### 2.4. `exercises`
Catálogo de ejercicios específicos que pertenecen a una disciplina deportiva.
- `id` (UUID, PK)
- `sport_id` (UUID, FK -> `sports.id` RESTRICT)
- `name` (VARCHAR 255): Nombre del ejercicio (e.g. "Press Banca", "Sentadilla").

### 2.5. `routines`
La plantilla abstracta o plan de entrenamiento recurrente que un usuario diseña.
- `id` (UUID, PK)
- `user_id` (UUID, FK -> `users.id` CASCADE): Autor/Dueño de la rutina.
- `sport_id` (UUID, FK -> `sports.id` RESTRICT): Disciplina a la que pertenece la rutina.
- `name` (VARCHAR 255): Nombre de la plantilla (e.g. "Día de Piernas", "Empuje").

### 2.6. `routine_exercises`
Tabla intermedia (Pivote) con orden lógico. Asigna qué ejercicios componen una rutina y en qué orden.
- `id` (UUID, PK)
- `routine_id` (UUID, FK -> `routines.id` CASCADE)
- `exercise_id` (UUID, FK -> `exercises.id` RESTRICT)
- `order_index` (INTEGER): Asegura que se respete el orden de ejecución diseñado por el usuario.
- **Constraint UNIQUE:** (routine_id, exercise_id). No repite el mismo ejercicio en la misma rutina (variantes usan entidades separadas de exercise_id de ser necesario).

### 2.7. `training_sessions`
La instancia real en el tiempo en la que un usuario decide realizar una rutina. Es el contenedor del progreso logueado de ese día.
- `id` (UUID, PK)
- `user_id` (UUID, FK -> `users.id` CASCADE): Identifica quién entrena.
- `routine_id` (UUID, FK -> `routines.id` CASCADE): Identifica qué rutina se está completando.
- `started_at` (TIMESTAMP WITH TIME ZONE): Cuándo inició.
- `ended_at` (TIMESTAMP WITH TIME ZONE, NULLABLE): Cuándo terminó (sirve para controlar sesiones en progreso o medir duración).

### 2.8. `session_exercises`
Instancia particular del ejercicio (snapshot) derivado de una `routine_exercises` particular que sirve como ancla para asociarle las métricas reales ("Sets de hoy").
- `id` (UUID, PK)
- `session_id` (UUID, FK -> `training_sessions.id` CASCADE)
- `exercise_id` (UUID, FK -> `exercises.id` RESTRICT)
- `order_index` (INTEGER)

### 2.9. `session_sets`
El logotipo anatómico granular de rendimiento. Almacena las repeticiones, el peso y si se completó o se falló el set previsto.
- `id` (UUID, PK)
- `session_exercise_id` (UUID, FK -> `session_exercises.id` CASCADE)
- `set_number` (INTEGER): 1er Set, 2do Set, etc.
- `reps` (INTEGER): Número de repeticiones realizadas.
- `weight_kg` (NUMERIC 5,2): Peso levantado.
- `completed` (BOOLEAN): Estado del completado del set (sirve para UI y UX de descanso).
- **Constraint UNIQUE:** (session_exercise_id, set_number).
