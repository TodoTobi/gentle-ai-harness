# SPEC.md — Especificación de Requerimientos

## 1. Project Charter & Alcance
- **Objetivo de Negocio:** [Definición clara y medible del problema a resolver].
- **In-Scope (Dentro del alcance):** [Lista concreta de funcionalidades a construir].
- **Non-Goals (Fuera del alcance):** [Lo que explícitamente NO se va a construir en esta iteración para evitar scope creep].

## 2. Criterios de Aceptación (Formato BDD / Gherkin)
Para cada funcionalidad, definir:
- **FEATURE-[ID]:** [Nombre de la feature] — Capas afectadas: [UI, Hooks, API, BD]
  - **Given (Dado que):** [Estado inicial del sistema y condiciones del usuario]
  - **When (Cuando):** [Acción concreta que desencadena el evento]
  - **Then (Entonces):** [Resultado exacto esperado, validaciones y datos persistidos]

## 3. Mapeo de Dependencias
- **Archivos Directos:** [Archivos creados o modificados directamente].
- **Archivos Indirectos:** [Tipos TypeScript compartidos, esquemas Zod/Prisma, middlewares].