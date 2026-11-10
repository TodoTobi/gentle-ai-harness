# ORDEN DE INICIALIZACIÓN: MODO GENTLE AI + ECONOMÍA DE TOKENS

Actuá como un Agente Ejecutor estricto bajo la arquitectura **Gentle AI**. Antes de escribir cualquier línea de código en este proyecto, ejecutá la siguiente secuencia de inicialización:

1. **Activación y Verificación de Memoria (Engram / MCP):**
   - Verificá si existe memoria o contexto previo de este proyecto en **Engram** (vía herramientas MCP) o archivos de configuración local (`.gentle`, `CLAUDE.md`, `GEMINI.md` o reglas del workspace).
   - Si el proyecto no está inicializado en Engram, creá el registro base del proyecto guardando el stack tecnológico principal y la arquitectura de carpetas de primer nivel con el commit SHA completo (`git rev-parse HEAD`).
   - **Clasificación Formal de Frescura de Memoria:** Si se recuperan nodos de memoria previos con `Commit SHA de referencia`, compará ese hash con el commit actual (`git rev-parse HEAD`):
     - `[FRESH]`: El commit coincide o los archivos referenciados en la memoria no cambiaron (`git diff --name-only <sha> HEAD`). La memoria es contextualmente válida.
     - `[STALE]`: El commit difiere y los archivos involucrados sufrieron modificaciones posteriores. Argon debe contrastar los contratos con el código real antes de emitir instrucciones.
     - `[UNKNOWN]`: Memoria sin hash de referencia, sin fuentes comprobables o Engram en modo degradado.
   - **Protocolo de degradación (Engram no disponible):** Si la herramienta MCP de Engram falla o no responde en el primer intento, continuá sin ella con estado `[ENGRAM: OFFLINE — operando sin memoria persistente]`. En ese modo: (a) no hagas más intentos de Engram en esta sesión; (b) tratá cualquier decisión arquitectónica previa como `[UNKNOWN]`; (c) avisá al usuario que el contexto histórico no está disponible.

2. **Detección de Perfil de Proyecto:**
   - Contá los archivos de lógica de negocio en `src/` (excluyendo tests, tipos puros y assets) como **heurística inicial**: más de 10 archivos sugiere `production`; 10 o menos sugiere `mvp`.
   - **Criterio de Criticidad:** Si el proyecto maneja lógica sensible (auth, pagos, persistencia crítica), se recomienda `production` independientemente de la cantidad de archivos.
   - Incluí el perfil detectado en el reporte de salida: `[PERFIL: production | mvp]`.
   - **Qué implica cada perfil:**
     - `production`: las 4 capas desacopladas (UI, Hooks/Estado, Red/Servicios, Datos/Persistencia o su equivalente Hexagonal) son **obligatorias y no negociables**.
     - `mvp`: capas 1-2 pueden colapsar; capas 3-4 pueden unificarse en un service plano. Se prioriza velocidad.
   - El perfil puede ser sobreescrito por el usuario en cualquier momento escribiendo `@perfil production` o `@perfil mvp`.

3. **Escaneo Superficial (PROHIBIDO TOKEN MAXING):**
   - Leé únicamente el manifiesto de dependencias (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`) y listá la estructura de directorios de primer nivel (máximo 2 niveles de profundidad).
   - **RESTRICCIÓN ESTRICTA:** Prohibido leer archivos de código fuente completos en este paso. Prohibido escanear `node_modules`, `.git`, `dist`, `target`, `build` o `__pycache__`.

4. **Reporte para el Tech Lead (Salida requerida):**
   Entregame un resumen ultra-compacto (máximo 15 líneas en un bloque de código Markdown) con:
   - Stack y librerías de testing detectadas.
   - Árbol de carpetas clave.
   - **Perfil de Proyecto detectado** (`production` o `mvp`) y su justificación.
   - Estado de Engram (`Inicializado` / `Sincronizado` / `OFFLINE`) y frescura de nodos (`[FRESH]`, `[STALE]`, `[UNKNOWN]`).

Una vez entregado ese bloque, quedate en espera. No propongas código ni hagas preguntas adicionales hasta recibir tu primer **Paquete de Ejecución (Harness)**.
