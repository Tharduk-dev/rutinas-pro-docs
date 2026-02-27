# Estándares de Documentación Oficial

El repositorio `rutinas-pro` mantiene una separación estricta entre la documentación orientada a Máquinas/Desarrolladores (DTE) y la orientación a Humanos (Manuales).

## 1. Documentación Técnica Extendida (DTE)
- **Audiencia:** Agentes de Inteligencia Artificial (Ejecutores, Auditores, etc.), Arquitectos y Desarrolladores.
- **Propósito:** Describir la arquitectura técnica, las clases, las funciones, las librerías, los contratos de API y los esquemas de bases de datos.
- **Ubicación:** 
  - `/docs/architecture/` (Lógica de dominio, patrones, estructura).
  - `/docs/api/` (Endpoints, Zod schemas, dependencias).
  - `/docs/database/` (Modelos relacionales, migraciones).
- **Plantilla Oficial:** Ver `docs/templates/technical_doc_template.md`.

## 2. Manuales de Usuario y Operación (Human Docs)
- **Audiencia:** Usuarios humanos, Product Owners, Administradores de Sistemas o Desarrolladores en fase de Onboarding (uso, no código).
- **Propósito:** Explicar cómo lanzar una aplicación, activar una configuración, configurar variables de entorno, o el paso a paso ("paso a paso") para usar una feature específica.
- **Ubicación:** 
  - `/docs/manuals/` (Ej. `setup_local.md`, `onboarding_humano.md`, `configuracion_app.md`).
- **Plantilla Oficial:** Ver `docs/templates/user_manual_template.md`.

## Regla de Oro
Ningún manual de usuario debe contener bloques extensos de código fuente, y ningún DTE técnico debe estar escrito en tono coloquial de tutorial paso a paso humano. Se mantienen independientes para garantizar que los Agentes no confundan guías de usuario con reglas estructurales del motor.
