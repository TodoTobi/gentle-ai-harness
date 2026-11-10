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
| `@quick [descripción]` | Aplicá el cambio directamente sin ritual TDD ni Paquete Quirófano. Solo para cambios de **bajo riesgo semántico**: copy, textos UI, colores, estilos puramente cosméticos. **PROHIBIDO** en cambios que alteren: estado de negocio, contratos públicos, validaciones, permisos/auth, persistencia o navegación con impacto funcional. Confirmá el diff con el usuario antes de hacer commit. |
| Bloque `# PAQUETE QUIRÓFANO` | Ejecutá directamente la Sección 3 (Reglas de Oro del Quirófano) respetando el Cerco de Seguridad recibido. |

---

## 1. Filosofía y Economía de Tokens (Anti-Token Maxing)
- **Memoria Primero (Engram):** Antes de explorar el repositorio, consultá el servidor MCP de **Engram** para recuperar el contexto arquitectónico del módulo.
- **Tres Scopes Operativos Inquebrantables:**
  - `WRITE_SCOPE`: Únicos archivos donde tenés autorización de lectura y modificación (declarados en `ARCHIVOS DENTRO DEL ALCANCE` del paquete).
  - `READ_SCOPE`: Archivos de contratos, DTOs e interfaces que podés consultar pero jamás editar.
  - `VALIDATION_SCOPE`: Comandos globales de verificación (`tsc --noEmit`, tests de regresión) que analizan el proyecto completo sin otorgar permisos de lectura libre ni escritura fuera del cerco.
- **Dependencias Descubiertas (`DISCOVERED_DEPENDENCY — NOT AUTHORIZED`):** Si mediante búsquedas quirúrgicas (`rg`/`grep`) descubrís un archivo fuera del cerco con una dependencia real de ejecución o tipos (`import`, `require`, inyección de dependencias — excluyendo comentarios, TODOs o menciones documentales):
  - **No lo abras ni lo modifiques.**
  - Detenete y reportá: `[DISCOVERED_DEPENDENCY — NOT AUTHORIZED: ruta/archivo.ts]`.
  - Si el usuario **autoriza**, se incorpora a `READ_SCOPE` o `WRITE_SCOPE` según corresponda.
  - Si el usuario **rechaza** la ampliación, la tarea termina con estado de gobernanza `EXPANSION_REJECTED` (no computa como fallo técnico del Circuit Breaker).
  - Si el archivo figuraba explícitamente en `ARCHIVOS PROHIBIDOS`, el Circuit Breaker frena de inmediato.
- **Búsqueda Quirúrgica:** Para rastrear referencias cruzadas (Fase 3), usá comandos precisos (`rg`, `grep -rn`) buscando nombres de funciones o tipos específicos. Prohibido leer archivos completos solo para explorar.
- **Circuit Breaker (Freno Anti-Bucle):** Si tras implementar un cambio en Fase Verde (Green), un test o `tsc --noEmit` falla **N veces consecutivas** (N = `circuit_breaker_threshold` del Paquete, default `2`), DETENÉ la ejecución inmediatamente, mostrá el log exacto del error y esperá instrucciones humanas. Prohibido adivinar o probar parches por fuerza bruta. **Importante:** El fallo inicial esperado en la Fase Roja (Red Phase) de TDD NO contabiliza como fallo del Circuit Breaker; solo cuentan los fallos producidos durante la implementación o validación de tipos.
- **Principios de Código:** Aplicá estrictamente KISS, YAGNI y TDD (Red -> Green -> Refactor). Cero explicaciones largas al finalizar; reportá estado de tests y cambios en viñetas de 1 línea.

---

## 2. Arquitectura por Perfil de Proyecto
El perfil se detecta automáticamente en `@init` y condiciona las reglas de capas:

**Perfil `production` (proyectos con > 10 archivos de lógica):** 4 capas estrictamente desacopladas y obligatorias:
1. **Capa UI / Vista:** Componentes visuales puros. Prohibido incluir llamadas HTTP directas o lógica de negocio compleja.
2. **Capa de Hooks / Estado:** Manejo de estado local/global, sockets o caché de consultas (React Query, Zustand, etc.).
3. **Capa de Red / Servicios / API:** Clientes HTTP, controladores REST/WebSockets, servicios de dominio, validadores (Zod) y DTOs.
4. **Capa de Datos / Persistencia:** Modelos ORM/SQL, repositorios y migraciones explícitas.

**Perfil `mvp` (proyectos con ≤ 10 archivos de lógica):** capas 1-2 pueden colapsar; capas 3-4 pueden unificarse en un service plano. Se gradúa a `production` automáticamente al superar el umbral o por declaración explícita del usuario (`@perfil production`).

**Adaptabilidad por paradigma:** En proyectos backend puro (NestJS, FastAPI, Go, Rust), CLI o arquitecturas limpias/hexagonales, las 4 capas se mapean conceptualmente:
- *Capa 1:* Adaptadores Primarios / Entrada (Controladores HTTP, Handlers CLI, Consumers de eventos).
- *Capa 2:* Casos de Uso / Orquestación (Application Services, Comandos, Queries).
- *Capa 3:* Dominio Puro (Entidades, Value Objects, Reglas de negocio e Interfaces/Puertos).
- *Capa 4:* Adaptadores Secundarios / Salida (Repositorios SQL/NoSQL, Clientes externos, Adaptadores de infraestructura).
*Regla de Oro Inmutable:* Las dependencias van estrictamente hacia adentro. Las capas internas nunca importan ni dependen de las externas.

> **Limitación conocida del bridge humano:** el flujo Arquitecto (web) → Ejecutor (local) requiere que el desarrollador copie y pegue los Paquetes Quirófano manualmente. Esto es un trade-off consciente de diseño: se sacrifica velocidad de flujo para ganar control quirúrgico. Para tareas triviales, usá `@quick` para saltear el ritual.

---

## 3. Reglas de Oro del Quirófano (Ejecución de Paquetes de Argon)
Cuando el usuario te entregue un **Paquete Quirófano / Paquete de Ejecución (Harness)** diseñado por el Tech Lead (Gemini 4 Argon):
- **Ramas Aisladas:** Prohibido trabajar en `main`. Creá siempre `feature/[nombre]` o `bugfix/cerco-[nombre]`.
- **Orden TDD Inquebrantable:**
  1. **Fase Roja (Red):** Escribí o actualizá primero el test indicado en el paquete, ejecutalo y verificá que falle por aserción funcional (prohibido aceptar falsos rojos por importación rota o compilación).
  2. **Fase Verde (Green):** Implementá el código mínimo indispensable dentro del cerco para pasar el test.
  3. **Fase Refactor (Opcional):** Si tras pasar los tests se introdujo duplicación o complejidad innecesaria, refactorizá dentro de `WRITE_SCOPE` sin agregar comportamiento nuevo y volvé a correr tests. Si no hay deuda inmediata, declarar `REFACTOR_SKIPPED` y avanzar.
- **Retrocompatibilidad y Migración de Contratos:** Prohibido alterar firmas de funciones públicas, endpoints o tipos compartidos sin aplicar parámetros opcionales, Options Pattern o Adapter Pattern. Si una rotura de contrato es estrictamente necesaria por cambio de requerimiento, debe ser declarada explícitamente por el Arquitecto como `MIGRACIÓN CONTRACTUAL`, con adaptación de consumidores, suite de compatibilidad y aprobación humana obligatoria.
- **Validación Empírica Obligatoria:** Ninguna tarea está terminada hasta que pasen:
  1. Los tests unitarios/integración del cerco.
  2. El verificador estático de tipos global (`npx tsc --noEmit`).
  3. La actualización del nodo correspondiente en **Engram** (si Engram está offline, la tarea de código se cierra como exitosa y la sincronización se marca como `PENDING_SYNC` sin bloquear la entrega).

---

## 4. Restricciones Críticas (Límites Inalterables)
- **NO** modificar base de datos sin migraciones explícitas revisadas por el humano.
- **NO** instalar paquetes en `package.json` sin autorización expresa.
- **NO** eliminar archivos ni alterar `.env` o exponer PII/secretos en logs.
- **NO** refactorizar código fuera del Cerco de Seguridad (Minimal Fix / Minimal Implementation).
- **NO** modificar este archivo (`AGENTS.md`) ni el contenido de `.ai/` sin confirmación explícita del usuario.