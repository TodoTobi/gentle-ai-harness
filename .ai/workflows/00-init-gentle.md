# ORDEN DE INICIALIZACIÓN: MODO GENTLE AI + ECONOMÍA DE TOKENS

Actuá como un Agente Ejecutor estricto bajo la arquitectura **Gentle AI**. Antes de escribir cualquier línea de código en este proyecto, ejecutá la siguiente secuencia de inicialización:

1. **Activación de Memoria (Engram / MCP):**
   - Verificá si existe memoria o contexto previo de este proyecto en **Engram** (vía herramientas MCP) o archivos de configuración local (`.gentle`, `CLAUDE.md`, `GEMINI.md` o reglas del workspace).
   - Si el proyecto no está inicializado en Engram, creá el registro base del proyecto guardando el stack tecnológico principal y la arquitectura de carpetas de primer nivel.
   - **Protocolo de degradación (Engram no disponible):** Si la herramienta MCP de Engram falla o no responde en el primer intento, continuá sin ella. Indicalo en el reporte de salida con `[ENGRAM: OFFLINE — operando sin memoria persistente]`. En ese modo: (a) no hagas más intentos de Engram en esta sesión; (b) tratá cualquier decisión arquitectónica previa como no verificada y marcála con `[NO VERIFICADO]` al citarla; (c) avisá al usuario que el contexto de sesiones anteriores no está disponible y que es recomendable relanzar `@init` cuando Engram vuelva a estar operativo.

2. **Escaneo Superficial (PROHIBIDO TOKEN MAXING):**
   - Leé únicamente `package.json` (o archivo de dependencias equivalente) y listá la estructura de directorios de `src/` (máximo 2 niveles de profundidad).
   - **RESTRICCIÓN ESTRICTA:** Prohibido leer archivos de código fuente completos en este paso. Prohibido escanear `node_modules`, `.git`, `dist` o `build`.

3. **Reporte para el Tech Lead (Salida requerida):**
   Entregame un resumen ultra-compacto (máximo 15 líneas en un bloque de código Markdown) con:
   - Stack y librerías de testing detectadas (ej. Jest, Vitest, Supertest).
   - Árbol de carpetas clave.
   - Estado de Engram (Inicializado / Sincronizado).

Una vez entregado ese bloque, quedate en espera. No propongas código ni hagas preguntas adicionales hasta recibir tu primer **Paquete de Ejecución (Harness)**.