# Política de Autenticación JWT

El sistema utiliza JSON Web Tokens (JWT) emitidos por Supabase Auth para la seguridad de los endpoints.

## Proceso de Validación
1. El cliente envía el token en el header `Authorization: Bearer <token>`.
2. El middleware en Fastify intercepta la petición.
3. Se valida la firma del token contra la `JWT_SECRET` del proyecto.
4. Se inyecta el `user_id` en el objeto de la petición para uso en servicios y repositorios.
