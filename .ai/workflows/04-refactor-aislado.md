# ORDEN DE QUIRÓFANO: REFACTORIZACIÓN DE COMPORTAMIENTO IDÉNTICO

Vamos a aplicar un Refactor Aislado sobre el archivo indicado por el usuario.

### REGLAS INQUEBRANTABLES (CONSERVACIÓN DE CONTRATO):
1. **Invarianza Externa:** La firma pública de las funciones exportadas, los tipos de entrada/salida y los efectos secundarios deben permanecer **100% idénticos**. Ningún archivo externo debe enterarse de este cambio.
2. **Cerco Cerrado:** Tenés prohibido modificar cualquier otro archivo fuera del archivo objetivo.

### SECUENCIA DE EJECUCIÓN OBLIGATORIA:
1. **Línea Base (Pre-Check):** Ejecutá los tests actuales de ese módulo y `npx tsc --noEmit`. Confirmá que todo está en verde antes de tocar una sola línea.
2. **Intervención:** Reducí la complejidad ciclomática aplicando Early Returns, extracción de funciones puras privadas y eliminación de código duplicado, respetando las reglas KISS y YAGNI de `AGENTS.md`.
3. **Verificación Determinista (Post-Check):** Volvé a correr exactamente los mismos tests y `npx tsc --noEmit`.
   - Si algún test pasa a rojo, **revertí el cambio inmediatamente** (`git checkout -- [archivo]`). Prohibido modificar el test para que el refactor pase.