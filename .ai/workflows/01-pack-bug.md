# MODO SCOUT: PRE-ARMADO DE PAQUETE DE BUG PARA TECH LEAD (GEMINI 4 ARGON)

Actuá como el subagente `@impact-scout`. **PROHIBIDO modificar archivos o intentar arreglar el bug todavía.** Tu único objetivo es investigar el problema reportado por el usuario con gasto mínimo de tokens y devolver la plantilla lista para enviarle al Arquitecto (Gemini 4 Argon en la web).

Ejecutá estrictamente estos pasos:
1. Consultá **Engram** y usá comandos rápidos (`rg` o `grep -rn`) para ubicar dónde se origina el síntoma y qué otros archivos consumen esa función/endpoint (máximo 50 líneas leídas en total, prohibido leer archivos enteros).
2. Devolveme **ÚNICAMENTE** un bloque de código Markdown con el siguiente texto ya rellenado con los datos reales de mi proyecto (para copiar y pegar en Gemini 4 Argon):

---
ROL: Staff Debugging Architect (Tech Lead).
OBJETIVO: Diseñar el "Cerco de Seguridad" y el Paquete Quirófano para que **Gemini 3.8 Flash** resuelva este bug en Antigravity bajo metodología Gentle AI.

### 1. ESCENARIO DEL BUG (GHERKIN / BDD GENERADO POR FLASH):
- **Dado que (Contexto):** [Completar según el código]
- **Cuando (Acción):** [Completar la acción que dispara el fallo]
- **Entonces (Esperado vs Real):** [Completar qué debía pasar vs el error exacto]
- **Capa sospechosa preliminar:** [Indicar si es UI, Hooks/Estado, Red/API o BD]

### 2. TRAZABILIDAD REAL DEL REPOSITORIO (OUTPUT DE RG/GREP):
- **Archivos Directos (Donde ocurre el fallo):** [Listar rutas exactas]
- **Archivos Indirectos (Consumidores que no debemos romper):** [Listar quiénes llaman a este código]
- **Firmas de Tipos/Funciones involucradas hoy:**
[Pegar acá solo las 10-20 líneas exactas de la firma actual de la función, interfaz TS o esquema involucrado]

---
INSTRUCCIÓN PARA ARGON:
Devolveme únicamente el **PAQUETE QUIRÓFANO PARA 3.8 FLASH** en un bloque Markdown copiable que contenga:
1. **Rama Git:** `bugfix/cerco-[nombre]`
2. **Cerco de Seguridad Estricto:** `ARCHIVOS PERMITIDOS` vs `ARCHIVOS PROHIBIDOS`.
3. **Contrato de Retrocompatibilidad:** Cómo arreglarlo sin romper los Archivos Indirectos listados arriba (parámetros opcionales, Options pattern o Adapter).
4. **Paso 1 (Fase Roja - TDD):** El código exacto del test que reproduce el bug hoy.
5. **Paso 2 (Fase Verde - Minimal Fix):** La instrucción quirúrgica para pasar el test sin refactorizar código vecino.
6. **Paso 3 (Cierre):** Comandos de test, `npx tsc --noEmit`, freno Circuit Breaker (máximo 2 intentos) y nota para guardar en **Engram**.