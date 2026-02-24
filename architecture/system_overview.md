# Visión General del Sistema

El sistema "Controla tus Rutinas" opera bajo un modelo de monorepo con servicios desacoplados pero integrados mediante contratos de API estrictos.

## Componentes Principales
1. **Mobile Flutter App**: Cliente principal para el usuario final. Implementa Clean Architecture separando capas de Data, Domain y Presentation por cada feature.
2. **Backend API (Fastify)**: Orquestador de lógica de negocio y persistencia.
3. **Worker Engine**: Procesamiento en segundo plano para cálculos de volumen y analíticas pesadas.
4. **Supabase**: Capa de persistencia y gestión de autenticación.

## Flujo de Datos
El cliente Flutter consume la API de Fastify mediante HTTPS. La API valida la identidad vía JWT y persiste los cambios en PostgreSQL. Los eventos asíncronos son gestionados por el Worker Engine.
