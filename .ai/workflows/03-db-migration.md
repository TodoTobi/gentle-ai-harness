# MODO SCOUT DB: PRE-ARMADO DE MIGRACIÓN SEGURA DE BASE DE DATOS

Extraé únicamente las tablas SQL o modelos de ORM actuales involucrados en el pedido del usuario (sin modificar nada todavía) y devolveme este bloque relleno para enviarle a Gemini 4 Argon:

---
ROL: Principal Database & Backend Architect.
CONTEXTO: Necesito modificar la capa de persistencia (Capa 4) para soportar el cambio solicitado.

### ESQUEMA ACTUAL EN EL PROYECTO:
[Flash: pegá acá solo los modelos/tablas actuales involucrados]

### CAMBIO SOLICITADO:
[Flash: describí el requerimiento de datos]

---
TAREA PARA ARGON (Generar Paquete de Migración Blindada para 3.8 Flash):
1. **Estrategia Expand-and-Contract:** Diseñá el cambio para que sea retrocompatible con el código actual (ej. columnas anulables con valores por defecto, nuevas tablas relacionales sin borrar columnas en uso).
2. **Validación de Integridad:** Qué índices (`INDEX`), claves foráneas (`FOREIGN KEY`) y restricciones (`CHECK` / `UNIQUE`) son obligatorios para evitar datos corruptos o queries lentas.
3. **Paquete Quirófano para Flash:**
   - Archivo de migración SQL/ORM exacto a generar.
   - Actualización de tipos/DTOs en la Capa 3 sin romper consumidores actuales.
   - Test de integración de persistencia (verificar inserción, lectura y violación de constraints).
   - Regla crítica: Prohibido ejecutar comandos destructivos (`DROP`, `TRUNCATE`, `migrate reset`) sin confirmación humana.