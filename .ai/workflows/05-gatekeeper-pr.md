# ORDEN DE AUDITORÍA FINAL PRE-MERGE (GATEKEEPER)

Antes de autorizar el cierre de esta rama o la creación del Pull Request, ejecutá una auditoría determinista e integral sobre todo el árbol de trabajo.

### 1. Auditoría Integral del Estado de Git (Three-Way Diff Inspection)
Ejecutá los siguientes tres comandos para no dejar cambios huérfanos o archivos no trackeados fuera del análisis:
1. `git status --porcelain`: inspeccioná cambios pendientes y archivos sin trackear (`??`). Si existen archivos residuales, temporales o fuera de `WRITE_SCOPE`, **DETENÉ EL GATEKEEPER** de inmediato:
   > `[GATEKEEPER BLOCKED] Se detectaron archivos no autorizados o residuales en el árbol de trabajo: [listar]`
2. `git diff --cached`: auditá los cambios preparados para commit.
3. `git diff [BASE_COMMIT_SHA] HEAD` (o `git diff main...HEAD` si no se dispone de SHA): auditá el diff total acumulado en la rama contra el commit base del paquete.

### 2. Checklist de Seguridad, Contratos y Limpieza:
- **Respeto Estricto de Scopes:** ¿Todos los archivos modificados pertenecen a `WRITE_SCOPE`? Prohibido autorizar PRs con cambios accidentales en archivos fuera de cerco.
- **Limpieza de Código:** ¿Se eliminaron `console.log` de depuración, mocks temporales, credenciales hardcodeadas, secretos o datos sensibles (PII)?
- **Invarianza de Dependencias:** ¿`package.json` o archivos lock permanecen intactos a menos que haya habido autorización humana expresa?
- **Separación de Capas:** ¿Se mantuvo el desacoplamiento arquitectónico según el perfil (`production` / `mvp`)?

### 3. Validación Empírica Final:
- Ejecutar verificación estática de tipos: `npx tsc --noEmit` (debe dar 0 errores).
- Ejecutar suite completa de tests de regresión: `npm test` o runner equivalente del proyecto. Confirmar que no hay regresiones nuevas respecto al baseline.

### 4. Entregable (Pull Request + Memoria Engram):
- **Sincronización en Engram:** Guardá en Engram el resumen de la decisión técnica vinculada al SHA completo de cierre (`git rev-parse HEAD`). Si Engram está offline, marcá el PR con `[ENGRAM: PENDING_SYNC]`.
- Imprimí un bloque Markdown listo para el PR con título Conventional Commits, descripción concisa y la matriz de control de calidad:

```markdown
## Matriz de Control de Calidad (Gatekeeper)
- [x] Funcionalidad / Escenario BDD cumplido y probado.
- [x] Cerco de Seguridad respetado (100% cambios dentro de WRITE_SCOPE).
- [x] Árbol de Git limpio (verificado con `git status --porcelain`, 0 archivos residuales).
- [x] Blindaje de Contratos y Retrocompatibilidad verificados.
- [x] Pruebas de Regresión Globales al 100% (cero fallos nuevos vs baseline).
- [x] Verificación de Tipos estricta (0 errores en `tsc --noEmit`).
- [x] Sincronización en Engram completada o documentada.
```
