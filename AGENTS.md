# AGENTS.md — Router Maestro y Contrato Operativo (Gentle AI + Cerco de Seguridad)

## 0. Tabla de Enrutamiento Dinámico (Lazy Loading de Prompts)
**REGLA CRÍTICA DE ECONOMÍA DE TOKENS:** Tenés **PROHIBIDO** leer los archivos dentro de `.ai/workflows/` o `.ai/agents/` al iniciar la sesión o por curiosidad. Solo debés abrir y ejecutar el archivo específico cuando el usuario escriba su comando disparador en el chat:

| Comando del Usuario | Acción Obligatoria de Gemini 3.8 Flash |
|---|---|
| `@init` | Leé y ejecutá estrictamente `.ai/workflows/00-init-gentle.md` |
| `@bug [descripción/error]` | Actuá como `.ai/agents/impact-scout.md` y ejecutá `.ai/workflows/01-pack-bug.md` |
| `@feature [requerimiento]` | Actuá como `.ai/agents/impact-scout.md` y ejecutá `.ai/workflows/02-pack-feature.md` |
| `@db [cambio de esquema]` | Leé y ejecutá estrictamente `.ai/workflows/03-db-migration.md` |
| `@refactor [ruta/archivo]` | Leé y ejecutá estrictamente `.ai/workflows/04-refactor-aislado.md` |
| `@cierre` o `@pr` | Auditá con `.ai/agents/code-reviewer.md`, si hay cambios de API usá `.ai/agents/documentador.md`, y ejecutá `.ai/workflows/05-gatekeeper-pr.md` |
| Bloque `# PAQUETE QUIRÓFANO` | Ejecutá directamente la Sección 3 (Reglas de Oro del Quirófano) respetando el Cerco de Seguridad recibido. |

---

## 1. Filosofía y Economía de Tokens (Anti-Token Maxing)
- **Memoria Primero (Engram):** Antes de explorar el repositorio, consultá el servidor MCP de **Engram** para recuperar el contexto arquitectónico del módulo.
- **Respeto Absoluto al Cerco de Seguridad:** Solo tenés permitido leer y modificar los archivos listados en `ARCHIVOS DENTRO DEL ALCANCE` (o `ARCHIVOS PERMITIDOS`) del Paquete de Ejecución actual.
- **Búsqueda Quirúrgica:** Para rastrear referencias cruzadas (Fase 3), usá comandos precisos (`rg`, `grep -rn`) buscando nombres de funciones o tipos específicos. Prohibido leer archivos completos solo para explorar.
- **Circuit Breaker (Freno Anti-Bucle):** Si tras implementar un cambio, un test o `tsc --noEmit` falla **2 veces consecutivas**, DETENÉ la ejecución inmediatamente, mostrá el log exacto del error y esperá instrucciones humanas. Prohibido adivinar o probar parches por fuerza bruta.
- **Principios de Código:** Aplicá estrictamente KISS, YAGNI y TDD (Red -> Green -> Refactor). Cero explicaciones largas al finalizar; reportá estado de tests y cambios en viñetas de 1 línea.

---

## 2. Arquitectura Estricta en 4 Capas Desacopladas
1. **Capa UI / Vista:** Componentes visuales puros. Prohibido incluir llamadas HTTP directas o lógica de negocio compleja.
2. **Capa de Hooks / Estado:** Manejo de estado local/global, sockets o caché de consultas (React Query, Zustand, etc.).
3. **Capa de Red / Servicios / API:** Clientes HTTP, controladores REST/WebSockets, servicios de dominio, validadores (Zod) y DTOs.
4. **Capa de Datos / Persistencia:** Modelos ORM/SQL, repositorios y migraciones explícitas.

---

## 3. Reglas de Oro del Quirófano (Ejecución de Paquetes de Argon)
Cuando el usuario te entregue un **Paquete Quirófano / Paquete de Ejecución (Harness)** diseñado por el Tech Lead (Gemini 4 Argon):
- **Ramas Aisladas:** Prohibido trabajar en `main`. Creá siempre `feature/[nombre]` o `bugfix/cerco-[nombre]`.
- **Orden TDD Inquebrantable:**
  1. **Fase Roja (Red):** Escribí o actualizá primero el test indicado en el paquete, ejecutalo y verificá que falle.
  2. **Fase Verde (Green):** Implementá el código mínimo indispensable dentro del cerco para pasar el test.
- **Retrocompatibilidad de Contratos:** Prohibido alterar firmas de funciones públicas, endpoints o tipos compartidos sin aplicar parámetros opcionales, Options Pattern o Adapter Pattern.
- **Validación Empírica Obligatoria:** Ninguna tarea está terminada hasta que pasen:
  1. Los tests unitarios/integración del cerco.
  2. El verificador estático de tipos global (`npx tsc --noEmit`).
  3. La actualización del nodo correspondiente en **Engram**.

---

## 4. Restricciones Críticas (Límites Inalterables)
- **NO** modificar base de datos sin migraciones explícitas revisadas por el humano.
- **NO** instalar paquetes en `package.json` sin autorización expresa.
- **NO** eliminar archivos ni alterar `.env` o exponer PII/secretos en logs.
- **NO** refactorizar código fuera del Cerco de Seguridad (Minimal Fix / Minimal Implementation).
- **NO** modificar este archivo (`AGENTS.md`) ni el contenido de `.ai/` sin confirmación explícita del usuario.