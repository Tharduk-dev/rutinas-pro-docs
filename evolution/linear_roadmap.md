# Roadmap Estratégico y Operativo (Formato Linear)

**Estado:** Aprobado
**Fecha Lógico-Base:** 2026-02-27
**Herramienta de Tracking Objetivo:** [Linear](#)

> [!CAUTION]
> **Transición de Fuente de Verdad:** Este documento actúa como la *Fuente de Verdad Maestra* del Roadmap ÚNICAMENTE durante la Fase 0 y Fase 0.5. Una vez completado e integrado el **EPIC 2 (Worker de Linear)**, este fichero quedará *DEPRECADO* como motor operativo y la única **Fuente de Verdad Absoluta** del Roadmap pasará a ser la propia plataforma de Linear a través de su API.

Este documento estructura el desarrollo funcional del proyecto "Controla tus Rutinas" utilizando la nomenclatura y jerarquía de Linear (Epic > Sub-Epic > Issue). Define desde la cimentación base hasta las fases avanzadas de escalado.

---

## EPIC 1: Cimentación Arquitectónica y Gobernanza (Fase 0)
**Linear ID:** RTN-E1
**Status:** In Progress
**Description:** Asentamiento de las bases documentales, del pipeline multi-agente y la normalización inicial del monolito antes del inicio intensivo del código.

### Sub-Epic 1.1: Ecosistema Multi-Agente
- [x] **[RTN-1]** Definir `governance_engine.md` y `governance_standards.md`
- [x] **[RTN-2]** Implementar Catálogo de Roles y Restricciones Estrictas de Prompt.
- [x] **[RTN-3]** Rediseño del Pipeline Asíncrono (Problem Statement Record - PSR).
- [x] **[RTN-4]** Auditar Simulación Human-in-the-loop y Zero Push Policy.

### Sub-Epic 1.2: Normalización de Deuda Técnica Inicial
- [x] **[RTN-5]** Sincronizar DTE de Base de Datos según esquema preexistente en Supabase.
- [x] **[RTN-6]** Sincronizar DTE Backend según inicialización preexistente en Fastify/Zod.
- [x] **[RTN-7]** Inicializar `project_state.yaml` reflejando el estado real actual de la base de código.

---

## EPIC 2: Ciclo de Integración CI/CD y Autonomía Agencial (Fase 0.5) 
**Linear ID:** RTN-E2
**Status:** In Progress
**Description:** Habilitar a los agentes (y al usuario humano) el control absoluto, seguimiento y creación de estados de desarrollo a través de un ecosistema que consuma de forma bi-direccional la API de Linear.

> [!IMPORTANT]  
> Este Epic es un pre-requisito estricto antes de iniciar el EPIC 3. Garatinza el "Roadmap awareness" de los orquestadores (Gestor/Coordinador).

### Sub-Epic 2.1: Infraestructura de Worker Python para Linear API
**Documentación de Implementación Paso a Paso:**
1. **[x] [RTN-8] Creación del Worker Python:** 
   - Generación de un nuevo servicio en `apps/worker_linear_agent/`.
   - Inicialización de entorno virtual (`requirements.txt` o `poetry`) incluyendo librerías `fastapi`, `uvicorn`, `requests` (para llamadas API externas de Linear) y gestores de variables de entorno.
2. **[ ] [RTN-9] Despliegue en Railway:**
   - Creación del `Dockerfile` base de Python hiper-reducido (alpine/slim).
   - Generación del archivo `.railwayignore`.
   - Documentación de inyección manual de `LINEAR_API_KEY` en el dashboard de Railway.
3. **[x] [RTN-10] Desarrollo de Endpoints Bridge:**
   - `POST /linear/issue`: Endpoint que el Gestor o scripts locales puedan golpear con un JSON básico (title, description, status) y que internamente resuelva las queries a GraphQL/REST de Linear creando el issue y devolviendo el UUID.
   - `PATCH /linear/issue/{id}`: Endpoint para transicionar el estado del issue (`Todo` -> `In Progress` -> `Ready for Review`).

### Sub-Epic 2.2: CLI/Programa Local de Consumo 
1. **[RTN-11] Script Interfaz Local:**
   - Creación de un script en NodeJS o Python (`infra/scripts/linear_agent_cli.js`) ejecutable por los agentes a través de la consola mediante `npm run linear:update`.
   - El script leerá el `project_state.yaml`, identificará la *Current Task* y golpeará al Worker de Railway o a la API de Linear directamente para sincronizar estados.

---

## EPIC 3: Fase 1 Core - Motor de Entrenamiento (Mobile & Backend)
**Linear ID:** RTN-E3
**Status:** Todo
**Description:** Desarrollo del flujo Mínimo Viable (MVP) para la gestión de rutinas, permitiendo a los usuarios tener sus perfiles, crear un plan de entrenamiento y loguear sesiones.

### Sub-Epic 3.1: Entorno Flutter Base y Clean Architecture
- [ ] **[RTN-12]** Scaffold de Clean Architecture en `apps/mobile_flutter`.
- [ ] **[RTN-13]** Integración de Riverpod, GoRouter y dependencias Core en pubspec.yaml.
- [ ] **[RTN-14]** Integración del SDK de Supabase Auth en capa de Datos.

### Sub-Epic 3.2: API Rutinas y Ejercicios (Backend Node/Fastify)
- [ ] **[RTN-15]** Desarrollo `endpoints` CRUD para la tabla `routines` y `routine_exercises`.
- [ ] **[RTN-16]** Middleware de seguridad JWT verificando sesión contra Supabase.

### Sub-Epic 3.3: Flow Frontend de Entrenamiento (UI/UX)
- [ ] **[RTN-17]** Pantalla de Dashboard "Mis Rutinas".
- [ ] **[RTN-18]** Wizard de creación estructurada de rutinas deportivas.
- [ ] **[RTN-19]** Pantalla de "Entrenamiento Activo" (Timer, Check general de repeticiones y descansos).

---

## EPIC 4: Evolución Personal y Dashboard Avanzado (Fase 2)
**Linear ID:** RTN-E4
**Status:** Backlog

### Sub-Epic 4.1: Progreso Físico Multimedia
- [ ] **[RTN-20]** Integración del Bucket R2 Cloudflare para carga de imágenes front end.
- [ ] **[RTN-21]** Endpoint Backend/Worker en Railway para optimización de imágenes (.WEBP 720p).

### Sub-Epic 4.2: Analítica y Rendimiento del Usuario
- [ ] **[RTN-22]** Componente chart en Flutter para historial de levantamiento (Kilos Totales vs Semana).

---

## EPIC 5: Sistema Multideporte y Red Social/Comunidad (Fase 3 & 4)
**Linear ID:** RTN-E5
**Status:** Backlog

- [ ] **[RTN-23]** Soporte Multi-tenant para entrenadores y jerarquías (Vincular planes del entrenador con el calendario del usuario regular).
- [ ] **[RTN-24]** Habilidad de compartir sesiones finalizadas a perfiles públicos / feed interno.
