# MODO SCOUT: PRE-ARMADO DE NUEVA FEATURE PARA TECH LEAD (GEMINI 4 ARGON)

Actuá como el subagente `@impact-scout`. **PROHIBIDO escribir código de implementación todavía.** Investigá con mínimo consumo de tokens (usando **Engram** y `rg`/`grep`) qué partes del proyecto se conectarán con la funcionalidad pedida por el usuario y devolvé **ÚNICAMENTE** un bloque Markdown rellenado listo para copiar y pegar en Gemini 4 Argon:

---
ROL: Staff Systems Architect (Tech Lead).
OBJETIVO: Diseñar el Cerco de Seguridad y el Paquete Quirófano para que **Gemini 3.8 Flash** implemente esta feature en Antigravity bajo metodología Gentle AI.

### 1. ESPECIFICACIÓN GHERKIN / BDD (REDACTADA POR FLASH):
- **Feature:** FEATURE-[ID]: [Título]
- **Given / When / Then:** [Redactar los criterios de aceptación formales basados en el pedido]
- **Non-Goals (Fuera de alcance):** [Qué cosas NO tocar en esta iteración]
- **Capas afectadas:** [Indicar cuáles de las 4 capas (UI, Estado, API, BD) se tocan]

### 2. MAPA DE CONEXIÓN CON EL PROYECTO ACTUAL (OUTPUT DE RG/ENGRAM):
- **Archivos nuevos sugeridos:** [Proponer rutas según la estructura actual del repo]
- **Archivos existentes a conectar (Directos e Indirectos):** [Listar qué rutas/archivos actuales se van a importar o extender]
- **Modelos / Interfaces actuales que debemos respetar:**
[Pegar acá las firmas de tipos TS, modelos Prisma/SQL o DTOs existentes con los que esta feature va a convivir]

---
INSTRUCCIÓN PARA ARGON:
Dividí la implementación en pasos atómicos y devolveme el **PAQUETE QUIRÓFANO (PASO 1)** en un bloque Markdown copiable para 3.8 Flash con:
1. **Rama Git:** `feature/[nombre]`
2. **Cerco de Archivos:** Archivos a CREAR, MODIFICAR y PROHIBIDOS.
3. **Contratos Inmutables:** Tipos TS, DTOs y firmas exactas a crear (extensión aditiva sin romper contratos previos).
4. **Paso 1 (Fase Roja - TDD):** El test unitario/integración de frontera que Flash debe crear y hacer fallar primero.
5. **Paso 2 (Fase Verde - Minimal Implementation):** Reglas directas para pasar el test cumpliendo KISS y YAGNI.
6. **Paso 3 (Cierre):** Comando `npx tsc --noEmit`, freno Circuit Breaker (2 intentos) y nota técnica para guardar en **Engram**.