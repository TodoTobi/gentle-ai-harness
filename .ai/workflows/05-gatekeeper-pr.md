# ORDEN DE AUDITORÍA FINAL PRE-MERGE (GATEKEEPER)

Antes de cerrar esta rama, ejecutá una auditoría determinista sobre los cambios realizados usando exclusivamente `git diff main...HEAD` y `git status`:

1. **Verificación del Cerco de Seguridad:** Listá todos los archivos modificados y confirmá que ninguno pertenece a las zonas prohibidas ni quedaron archivos basura/temporales sin trackear.
2. **Checklist de Seguridad y Limpieza:**
   - ¿Se filtró algún `console.log` de depuración, comentario muerto, credencial hardcodeada o dato sensible (PII)?
   - ¿Se alteró `package.json` sin autorización?
3. **Ejecución Final:** Corré `npx tsc --noEmit` y la suite completa de tests de regresión del proyecto.
4. **Entregable (Pull Request + Memoria Engram):**
   - Si los puntos 1, 2 y 3 pasan con 0 errores, actualizá el nodo correspondiente en **Engram** con las decisiones arquitectónicas de esta rama (máximo 5 líneas).
   - Imprimime un bloque Markdown con el título del Commit (formato Conventional Commits), la descripción del Pull Request y esta **Matriz de Control de Calidad** verificada:
     - [x] Funcionalidad / Escenario BDD cumplido.
     - [x] Cerco de Seguridad respetado (0 archivos fuera de alcance).
     - [x] Blindaje Anti-Regresiones (Contratos externos intactos).
     - [x] Pruebas de Regresión Globales al 100% (0 fallos).
     - [x] Validación de Tipos (`tsc --noEmit`) limpia.
     - [x] Minimal Fix / Implementation sin deuda técnica.