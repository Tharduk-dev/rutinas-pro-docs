---
description: Breve descripción técnica del componente o dominio (Ej: Contratos API del Worker Linear)
---

# TÍTULO DEL COMPONENTE TÉCNICO

## 1. Propósito y Alcance
[Explica el "por qué" existe este componente a nivel técnico. No incluyas tutoriales de uso, limítate a su función dentro de la Clean Architecture o la infraestructura.]

## 2. Puntos Intelectuales (Archivos Relevantes)
- `ruta/al/archivo_principal.ts`: [Breve descripción de su responsabilidad]
- `ruta/al/modelo_base.ts`: [Breve descripción]

## 3. Clases, Funciones o Interfaces Core
[Lista de forma rigurosa las interfaces o firmas de funciones que otros módulos consumirán. Evita copiar el código fuente completo, enfócate en los contratos].

### Interfaz / Modelo: `NombreModelo`
- **Campo A:** Tipo - Descripción.
- **Campo B:** Tipo - Descripción.

### Endpoint / Método Principal: `AccionCore`
- **Input:** [Esquema de entrada, Zod / DTO]
- **Output:** [Esquema de salida]
- **Responsabilidad:** [Qué hace a nivel de datos]

## 4. Dependencias Críticas
- **Librería Externa:** [Ej: Zod, Fastify]
- **Infraestructura:** [Ej: Base de Datos, Queue]

## 5. Reglas de Modificación (Agentes)
[Instrucciones si un Ejecutor necesita alterar este documento o el código relacionado. Qué está prohibido tocar sin autorización].
