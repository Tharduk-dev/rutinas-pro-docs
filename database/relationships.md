# Modelo Relacional y Vínculos

## Relaciones Clave
1. **Profiles ↔ Users**: Relación 1:1 mediante `id` (FK de auth.users).
2. **Workouts ↔ Profiles**: Relación 1:N. Un perfil puede crear múltiples rutinas.
3. **Workout_Exercises**: Relación N:M entre `Workouts` y `Exercises`. Incluye metadatos de carga (sets, reps, rest).
4. **Logs ↔ Workout_Exercises**: Relación 1:N. Cada entrada de log referencia qué ejercicio de qué rutina se ejecutó.
