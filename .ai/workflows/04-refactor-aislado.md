# ORDEN DE QUIRÓFANO: REFACTORIZACIÓN SEGURA Y AISLADA

Vamos a aplicar una refactorización preservando comportamiento idéntico sobre el archivo o módulo indicado.

### CLASIFICACIÓN DE ALCANCE:
- **`LOCAL_REFACTOR` (Default):** Transformación interna de un único archivo. Invarianza externa del 100%, cerco cerrado a 1 archivo.
- **`MODULE_REFACTOR`:** Reestructuración coordinada de varios archivos del mismo módulo (requiere Paquete Quirófano formal).
- **`ARCHITECTURAL_REFACTOR`:** Modificación de límites o contratos entre módulos (requiere descomposición en pasos atómicos por el Arquitecto).

---

### REGLAS INQUEBRANTABLES:
1. **Invarianza Externa:** La firma pública, tipos de entrada/salida y efectos secundarios deben permanecer **100% idénticos**. Ningún consumidor externo debe enterarse del cambio.
2. **Cerco Cerrado:** Tenés prohibido modificar archivos fuera del alcance declarado.
3. **Protección Anti-Pérdida de Datos (Pre-Check de Git):**
   - Ejecutá `git status --porcelain [archivo]`.
   - Si el archivo contiene modificaciones previas sin commitear realizadas por el desarrollador, **DETENÉ LA EJECUCIÓN**:
     > `[DIRTY STATE DETECTED] El archivo [ruta] tiene cambios locales previos sin guardar. Commiteá o hacé stash de tus cambios antes de iniciar el refactor.`

---

### SECUENCIA DE EJECUCIÓN OBLIGATORIA:

1. **Línea Base y Baseline de Tests:**
   - Ejecutá el suite de tests del módulo y `npx tsc --noEmit`.
   - Si existen fallos preexistentes ajenos al refactor, registralos como `known_failures`.
   - Criterio de aceptación posterior: **cero fallos nuevos** (`new_failures == 0`).
2. **Intervención Quirúrgica:**
   - Reducí la complejidad ciclomática aplicando Early Returns, extracción de funciones privadas puras y eliminación de duplicación bajo principios KISS y YAGNI.
3. **Verificación Determinista (Post-Check):**
   - Volvé a correr exactamente los mismos tests y `npx tsc --noEmit`.
   - Si algún test que antes pasaba pasa a rojo:
     - **Reversión segura:** Restaurá el archivo a su estado limpio inicial registrado en el Pre-Check (`git checkout -- [archivo]` solo si el pre-check validó que no había cambios de usuario previos).
     - **Prohibido** modificar los tests para forzar que el refactor pase.
   - Reportá estado en una sola línea: `Refactor completado: tests verdes y cero regresiones en tipos.`
