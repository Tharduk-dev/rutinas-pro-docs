# Arquitectura del Backend

El backend está construido sobre **Fastify**, priorizando el bajo overhead y la velocidad de respuesta.

## Estructura de Capas (S-C-R Pattern)
Para garantizar la mantenibilidad, seguimos el flujo:
`Route -> Controller -> Service -> Repository`

- **Routes**: Definición de endpoints y esquemas de validación (Zod).
- **Controllers**: Manejo de la petición HTTP, extracción de datos y respuesta.
- **Services**: Lógica de negocio pura. No conocen el protocolo de transporte.
- **Repositories**: Interacción directa con la base de datos (PostgreSQL/Supabase).
- **Modules**: Agrupación lógica de las capas anteriores por dominio de negocio (p.ej., Auth, Workout).
