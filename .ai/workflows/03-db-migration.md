# MODO SCOUT DB: PRE-ARMADO DE MIGRACIÓN SEGURA DE BASE DE DATOS

Extraé únicamente las tablas SQL o modelos de ORM actuales involucrados en el pedido del usuario (sin modificar nada todavía) y devolveme este bloque relleno para enviarle a Gemini 4 Argon:

---
ROL: Principal Database & Backend Architect.
CONTEXTO: Necesito modificar la capa de persistencia (Capa 4) para soportar el cambio solicitado.

### PRE-ANÁLISIS OBLIGATORIO (completar antes de diseñar cualquier migración):
- **Motor de base de datos:** [PostgreSQL / MySQL / SQLite / otra] — versión: [X.Y]
- **Volumen estimado de filas** en las tablas afectadas: [ej. ~2M rows en `users`]
- **Riesgo de lock:** ¿el cambio requiere `ALTER TABLE` sobre columnas existentes? [Sí / No] — Si es Sí, Argon debe indicar si el motor soporta operaciones online/concurrent (ej. `ALTER TABLE ... ALGORITHM=INSTANT` en MySQL 8+, o índices `CONCURRENTLY` en PostgreSQL).
- **Estrategia de backfill:** si se agrega una columna NOT NULL o se migran datos, ¿hay un plan por lotes para evitar timeouts en tablas grandes? [Describir o indicar N/A]
- **Plan de rollback:** ¿qué comando revierte la migración sin pérdida de datos? [Describir]

### ESQUEMA ACTUAL EN EL PROYECTO:
[Flash: pegá acá solo los modelos/tablas actuales involucrados, incluyendo índices y constraints existentes]

### CAMBIO SOLICITADO:
[Flash: describí el requerimiento de datos]

---
TAREA PARA ARGON (Generar Paquete de Migración Blindada para 3.8 Flash):
1. **Análisis de riesgo de producción:** Basándote en el motor, versión y volumen declarados, determiná si la operación puede causar locks, timeouts o incompatibilidades con instancias del backend en versiones anteriores. Si el riesgo es alto, diseñá la migración en pasos atómicos separados.
2. **Estrategia Expand-and-Contract:** Diseñá el cambio para que sea retrocompatible con el código actual (ej. columnas anulables con valores por defecto, nuevas tablas relacionales sin borrar columnas en uso).
3. **Validación de Integridad:** Qué índices (`INDEX`), claves foráneas (`FOREIGN KEY`) y restricciones (`CHECK` / `UNIQUE`) son obligatorios para evitar datos corruptos o queries lentas.
4. **Paquete Quirófano para Flash:**
   - Archivo de migración SQL/ORM exacto a generar.
   - Actualización de tipos/DTOs en la Capa 3 sin romper consumidores actuales.
   - Test de integración de persistencia (verificar inserción, lectura y violación de constraints).
   - Regla crítica: Prohibido ejecutar comandos destructivos (`DROP`, `TRUNCATE`, `migrate reset`) sin confirmación humana.