# SQL Performance / Rendimiento en SQL

## Índice / Table of Contents

- [1. Indexes](#1-indexes)
- [2. Query Optimization](#2-query-optimization)
- [3. Execution Plans](#3-execution-plans)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)

---

<details>
<summary><strong>1. Indexes</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
Indexes are special data structures that improve the speed of data retrieval operations on a database table. They work like a book's index, allowing the database to find rows faster without scanning the whole table.

- **Purpose:** Speed up SELECT queries and sometimes speed up JOINs and WHERE clauses.
- **Types:**
  - Primary index (usually on the primary key)
  - Unique index (enforces uniqueness)
  - Composite index (multiple columns)
  - Full-text, spatial, and other specialized indexes
- **Syntax:**
  ```sql
  CREATE INDEX idx_name ON table(column);
  DROP INDEX idx_name;
  ```

#### Example
Suppose you have a large `employees` table and often search by `department`:
```sql
CREATE INDEX idx_department ON employees(department);
SELECT * FROM employees WHERE department = 'IT';
```

#### Visualization
Without an index, the database scans every row. With an index, it jumps directly to the relevant rows.

#### Use Cases
- Large tables with frequent searches on specific columns
- Columns used in WHERE, JOIN, or ORDER BY

#### Best Practices
- Index columns that are frequently filtered or joined
- Keep indexes as small as possible (avoid unnecessary columns)
- Monitor and remove unused indexes

#### Common Mistakes
- Over-indexing (too many indexes slow down INSERT/UPDATE/DELETE)
- Indexing columns with low selectivity (many repeated values)
- Forgetting to rebuild or update indexes after large data changes

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Los índices son estructuras especiales que mejoran la velocidad de las operaciones de búsqueda en una tabla. Funcionan como el índice de un libro, permitiendo a la base de datos encontrar filas rápidamente sin escanear toda la tabla.

- **Propósito:** Acelerar consultas SELECT y, a veces, JOINs y cláusulas WHERE.
- **Tipos:**
  - Índice primario (usualmente en la clave primaria)
  - Índice único (garantiza unicidad)
  - Índice compuesto (varias columnas)
  - Índices de texto completo, espaciales y otros especializados
- **Sintaxis:**
  ```sql
  CREATE INDEX idx_nombre ON tabla(columna);
  DROP INDEX idx_nombre;
  ```

#### Ejemplo
Supón que tienes una tabla grande `employees` y buscas frecuentemente por `department`:
```sql
CREATE INDEX idx_department ON employees(department);
SELECT * FROM employees WHERE department = 'IT';
```

#### Visualización
Sin índice, la base de datos recorre todas las filas. Con índice, salta directamente a las filas relevantes.

#### Casos de uso
- Tablas grandes con búsquedas frecuentes en columnas específicas
- Columnas usadas en WHERE, JOIN u ORDER BY

#### Buenas Prácticas
- Indexa columnas que se filtran o unen frecuentemente
- Mantén los índices lo más pequeños posible (evita columnas innecesarias)
- Monitorea y elimina índices no usados

#### Errores Comunes
- Exceso de índices (demasiados índices ralentizan INSERT/UPDATE/DELETE)
- Indexar columnas con baja selectividad (muchos valores repetidos)
- Olvidar reconstruir o actualizar índices tras grandes cambios de datos

</details>
</details>

<details>
<summary><strong>2. Query Optimization</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Query optimization is the process of making SQL queries run as efficiently as possible. This involves writing clear queries, using indexes, and understanding how the database engine works.

- **Purpose:** Reduce query execution time and resource usage.
- **Techniques:**
  - Use SELECT only for needed columns (avoid SELECT *)
  - Filter early with WHERE
  - Use JOINs efficiently
  - Avoid unnecessary subqueries
  - Use LIMIT/TOP for large result sets
  - Analyze and rewrite slow queries

#### Example
```sql
-- Inefficient
SELECT * FROM employees;

-- Optimized
SELECT name, department FROM employees WHERE department = 'IT';
```

#### Visualization
Optimized queries scan fewer rows and return only the necessary data.

#### Use Cases
- Reporting and analytics on large datasets
- Real-time dashboards
- Batch processing

#### Best Practices
- Profile queries with EXPLAIN/EXPLAIN PLAN
- Refactor queries that use too many subqueries or nested SELECTs
- Use appropriate data types and constraints

#### Common Mistakes
- Using SELECT * in production
- Not using indexes or filtering
- Ignoring query execution plans

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La optimización de consultas es el proceso de hacer que las consultas SQL sean lo más eficientes posible. Esto implica escribir consultas claras, usar índices y entender cómo funciona el motor de la base de datos.

- **Propósito:** Reducir el tiempo de ejecución y el uso de recursos.
- **Técnicas:**
  - Usar SELECT solo para las columnas necesarias (evitar SELECT *)
  - Filtrar pronto con WHERE
  - Usar JOINs eficientemente
  - Evitar subconsultas innecesarias
  - Usar LIMIT/TOP para grandes resultados
  - Analizar y reescribir consultas lentas

#### Ejemplo
```sql
-- Ineficiente
SELECT * FROM employees;

-- Optimizada
SELECT name, department FROM employees WHERE department = 'IT';
```

#### Visualización
Las consultas optimizadas recorren menos filas y devuelven solo los datos necesarios.

#### Casos de uso
- Reportes y análisis en grandes volúmenes de datos
- Dashboards en tiempo real
- Procesos batch

#### Buenas Prácticas
- Perfila consultas con EXPLAIN/EXPLAIN PLAN
- Refactoriza consultas con demasiadas subconsultas o SELECTs anidados
- Usa tipos de datos y restricciones apropiados

#### Errores Comunes
- Usar SELECT * en producción
- No usar índices ni filtros
- Ignorar los planes de ejecución

</details>
</details>

<details>
<summary><strong>3. Execution Plans</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
An execution plan is a visual or textual representation of how the database will execute a query. It shows the steps, order, and methods used to access and join tables.

- **Purpose:** Understand and optimize how queries are executed.
- **How to view:**
  - Use `EXPLAIN` (MySQL, PostgreSQL)
  - Use `EXPLAIN PLAN` (Oracle, SQL Server)
- **Key elements:**
  - Table scans vs. index scans
  - Join methods (nested loop, hash join, merge join)
  - Estimated cost and row counts

#### Example
```sql
EXPLAIN SELECT name FROM employees WHERE department = 'IT';
```

#### Visualization
Execution plans help identify bottlenecks, missing indexes, and inefficient joins.

#### Use Cases
- Diagnosing slow queries
- Tuning database performance

#### Best Practices
- Regularly review execution plans for critical queries
- Add or adjust indexes based on plan analysis
- Look for full table scans and optimize them

#### Common Mistakes
- Ignoring execution plans
- Misinterpreting plan output
- Focusing only on cost, not on row counts or join types

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un plan de ejecución es una representación visual o textual de cómo la base de datos ejecutará una consulta. Muestra los pasos, el orden y los métodos usados para acceder y unir tablas.

- **Propósito:** Entender y optimizar cómo se ejecutan las consultas.
- **Cómo verlo:**
  - Usar `EXPLAIN` (MySQL, PostgreSQL)
  - Usar `EXPLAIN PLAN` (Oracle, SQL Server)
- **Elementos clave:**
  - Escaneos de tabla vs. escaneos de índice
  - Métodos de join (nested loop, hash join, merge join)
  - Coste estimado y conteo de filas

#### Ejemplo
```sql
EXPLAIN SELECT name FROM employees WHERE department = 'IT';
```

#### Visualización
Los planes de ejecución ayudan a identificar cuellos de botella, índices faltantes y joins ineficientes.

#### Casos de uso
- Diagnóstico de consultas lentas
- Ajuste de rendimiento de la base de datos

#### Buenas Prácticas
- Revisa regularmente los planes de ejecución de consultas críticas
- Agrega o ajusta índices según el análisis del plan
- Busca escaneos completos de tabla y optimízalos

#### Errores Comunes
- Ignorar los planes de ejecución
- Malinterpretar la salida del plan
- Fijarse solo en el coste y no en el conteo de filas o tipo de join

</details>
</details>

<details>
<summary><strong>4. Advanced Index Types</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Beyond basic indexes, modern databases support advanced index types for specific scenarios.

- **Composite Index:** Index on multiple columns. Useful for queries filtering or sorting by several columns.
- **Unique Index:** Ensures all values in the indexed column(s) are unique.
- **Partial/Filtered Index:** Indexes only a subset of rows (e.g., WHERE active = 1).
- **Full-Text Index:** Optimized for searching text data (e.g., `CONTAINS`, `MATCH`).
- **Spatial Index:** For geographic data (GIS).

#### Example: Composite Index
```sql
CREATE INDEX idx_dept_salary ON employees(department, salary);
```

#### Example: Filtered Index
```sql
CREATE INDEX idx_active ON users(is_active) WHERE is_active = 1;
```

#### Best Practices
- Match composite index column order to query patterns.
- Use filtered indexes for sparse data.

#### Common Mistakes
- Creating composite indexes with unused columns.
- Overusing full-text indexes for non-text data.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Además de los índices básicos, las bases de datos modernas soportan tipos avanzados para escenarios específicos.

- **Índice compuesto:** Sobre varias columnas. Útil para consultas que filtran u ordenan por varias columnas.
- **Índice único:** Garantiza que todos los valores sean únicos.
- **Índice parcial/filtrado:** Solo para un subconjunto de filas (ejemplo: WHERE activo = 1).
- **Índice de texto completo:** Optimizado para búsquedas de texto (`CONTAINS`, `MATCH`).
- **Índice espacial:** Para datos geográficos (GIS).

#### Ejemplo: Índice compuesto
```sql
CREATE INDEX idx_dept_salary ON employees(department, salary);
```

#### Ejemplo: Índice filtrado
```sql
CREATE INDEX idx_activo ON users(is_active) WHERE is_active = 1;
```

#### Buenas Prácticas
- Ordena las columnas del índice compuesto según los patrones de consulta.
- Usa índices filtrados para datos dispersos.

#### Errores Comunes
- Crear índices compuestos con columnas no usadas.
- Abusar de índices de texto completo para datos no textuales.

</details>
</details>

<details>
<summary><strong>5. Statistics and Maintenance</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Statistics help the query optimizer estimate row counts and choose the best execution plan. Indexes and statistics require maintenance for optimal performance.

- **Updating statistics:**
  ```sql
  UPDATE STATISTICS table_name;
  ANALYZE TABLE table_name;
  ```
- **Rebuilding/Reorganizing indexes:**
  ```sql
  ALTER INDEX idx_name REBUILD;
  ALTER INDEX idx_name REORGANIZE;
  ```

#### Best Practices
- Update statistics after large data changes.
- Rebuild/reorganize indexes periodically.

#### Common Mistakes
- Ignoring statistics (leads to poor plans).
- Never maintaining indexes (causes fragmentation).

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Las estadísticas ayudan al optimizador a estimar conteos de filas y elegir el mejor plan de ejecución. Los índices y estadísticas requieren mantenimiento para un rendimiento óptimo.

- **Actualizar estadísticas:**
  ```sql
  UPDATE STATISTICS nombre_tabla;
  ANALYZE TABLE nombre_tabla;
  ```
- **Reconstruir/reorganizar índices:**
  ```sql
  ALTER INDEX idx_nombre REBUILD;
  ALTER INDEX idx_nombre REORGANIZE;
  ```

#### Buenas Prácticas
- Actualiza estadísticas tras grandes cambios de datos.
- Reconstruye/reorganiza índices periódicamente.

#### Errores Comunes
- Ignorar estadísticas (genera malos planes).
- No mantener los índices (causa fragmentación).

</details>
</details>

<details>
<summary><strong>6. Locking and Concurrency</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Locking controls how multiple users access data at the same time. Poor locking can cause deadlocks and slow performance.

- **Types:**
  - Row-level, page-level, table-level locks
  - Shared, exclusive, update locks
- **Transaction isolation levels:**
  - READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE

#### Example
```sql
BEGIN TRANSACTION;
UPDATE employees SET salary = salary + 1000 WHERE department = 'IT';
-- This row is now locked until COMMIT/ROLLBACK
COMMIT;
```

#### Best Practices
- Keep transactions short.
- Use the lowest isolation level that meets requirements.
- Monitor for deadlocks.

#### Common Mistakes
- Long transactions holding locks.
- Not handling deadlocks.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
El locking controla cómo varios usuarios acceden a los datos simultáneamente. Un mal manejo puede causar deadlocks y bajo rendimiento.

- **Tipos:**
  - Locks a nivel de fila, página, tabla
  - Locks compartidos, exclusivos, de actualización
- **Niveles de aislamiento de transacciones:**
  - READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE

#### Ejemplo
```sql
BEGIN TRANSACTION;
UPDATE employees SET salary = salary + 1000 WHERE department = 'IT';
-- Esta fila queda bloqueada hasta COMMIT/ROLLBACK
COMMIT;
```

#### Buenas Prácticas
- Mantén las transacciones cortas.
- Usa el menor nivel de aislamiento que cumpla los requisitos.
- Monitorea deadlocks.

#### Errores Comunes
- Transacciones largas que mantienen locks.
- No manejar deadlocks.

</details>
</details>

<details>
<summary><strong>7. Caching and Buffers</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Databases use memory to cache data and query results, reducing disk access and improving speed.

- **Buffer pool:** Stores recently accessed data pages.
- **Query cache:** Stores results of recent queries (if enabled).

#### Best Practices
- Size buffer pools appropriately for workload.
- Monitor cache hit ratios.

#### Common Mistakes
- Relying on cache for correctness (always validate data freshness).
- Not tuning memory settings for large databases.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Las bases de datos usan memoria para cachear datos y resultados de consultas, reduciendo el acceso a disco y mejorando la velocidad.

- **Buffer pool:** Almacena páginas de datos accedidas recientemente.
- **Query cache:** Almacena resultados de consultas recientes (si está habilitado).

#### Buenas Prácticas
- Dimensiona el buffer pool según la carga de trabajo.
- Monitorea el ratio de aciertos de caché.

#### Errores Comunes
- Confiar en el caché para la validez de los datos (siempre valida la frescura).
- No ajustar la memoria para bases de datos grandes.

</details>
</details>

<details>
<summary><strong>8. Join Optimization</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Efficient joins are critical for performance, especially with large tables.

- **Join types:** Nested loop, hash join, merge join
- **Indexing join columns:** Indexes on join columns speed up lookups
- **Join order:** The order of tables can affect performance

#### Example
```sql
SELECT e.name, d.department
FROM employees e
JOIN departments d ON e.department_id = d.id;
```

#### Best Practices
- Index columns used in joins
- Use the appropriate join type for data size and distribution
- Analyze execution plans for join performance

#### Common Mistakes
- Joining large tables without indexes
- Using the wrong join type for the scenario

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Joins eficientes son clave para el rendimiento, especialmente con tablas grandes.

- **Tipos de join:** Nested loop, hash join, merge join
- **Indexar columnas de join:** Los índices en columnas de join aceleran las búsquedas
- **Orden de join:** El orden de las tablas puede afectar el rendimiento

#### Ejemplo
```sql
SELECT e.name, d.department
FROM employees e
JOIN departments d ON e.department_id = d.id;
```

#### Buenas Prácticas
- Indexa las columnas usadas en joins
- Usa el tipo de join adecuado para el tamaño y distribución de los datos
- Analiza los planes de ejecución para el rendimiento de joins

#### Errores Comunes
- Unir tablas grandes sin índices
- Usar el tipo de join incorrecto para el escenario

</details>
</details>

<details>
<summary><strong>9. Monitoring Tools</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Monitoring tools help track query performance, resource usage, and identify bottlenecks.

- **Native tools:**
  - SQL Server Profiler, MySQL Workbench, EXPLAIN ANALYZE
- **External tools:**
  - pgAdmin, Percona Toolkit, DataDog, New Relic

#### Best Practices
- Regularly monitor slow queries and resource usage
- Set up alerts for unusual activity

#### Common Mistakes
- Not monitoring production systems
- Ignoring warning signs (slow queries, high CPU/memory)

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Las herramientas de monitoreo ayudan a rastrear el rendimiento de consultas, uso de recursos e identificar cuellos de botella.

- **Herramientas nativas:**
  - SQL Server Profiler, MySQL Workbench, EXPLAIN ANALYZE
- **Herramientas externas:**
  - pgAdmin, Percona Toolkit, DataDog, New Relic

#### Buenas Prácticas
- Monitorea regularmente consultas lentas y uso de recursos
- Configura alertas para actividad inusual

#### Errores Comunes
- No monitorear sistemas en producción
- Ignorar señales de advertencia (consultas lentas, alto uso de CPU/memoria)

</details>
</details>

<details>
<summary><strong>10. Performance Anti-Patterns</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Certain coding patterns consistently lead to poor performance. Avoiding these anti-patterns is key to scalable SQL.

- **N+1 Query Problem:** Running a query in a loop instead of using a JOIN
- **SELECT in loops:** Fetching data row-by-row instead of set-based operations
- **Functions in WHERE:** Using functions on indexed columns disables index usage
- **Overusing DISTINCT:** Hides data issues and adds unnecessary work

#### Example: N+1 Problem
```sql
-- Bad
FOR EACH order_id IN orders:
  SELECT * FROM order_items WHERE order_id = ...;
-- Good
SELECT * FROM orders o JOIN order_items oi ON o.id = oi.order_id;
```

#### Best Practices
- Use set-based operations instead of row-by-row
- Avoid functions on indexed columns in WHERE
- Use DISTINCT only when necessary

#### Common Mistakes
- Writing procedural code in SQL
- Using SELECT in application loops

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Ciertos patrones de código llevan siempre a bajo rendimiento. Evitar estos anti-patrones es clave para SQL escalable.

- **Problema N+1:** Ejecutar una consulta en un bucle en vez de usar JOIN
- **SELECT en bucles:** Obtener datos fila por fila en vez de operaciones en conjunto
- **Funciones en WHERE:** Usar funciones en columnas indexadas deshabilita el índice
- **Abusar de DISTINCT:** Oculta problemas de datos y agrega trabajo innecesario

#### Ejemplo: Problema N+1
```sql
-- Mal
PARA CADA order_id EN orders:
  SELECT * FROM order_items WHERE order_id = ...;
-- Bien
SELECT * FROM orders o JOIN order_items oi ON o.id = oi.order_id;
```

#### Buenas Prácticas
- Usa operaciones en conjunto en vez de fila por fila
- Evita funciones en columnas indexadas en WHERE
- Usa DISTINCT solo cuando sea necesario

#### Errores Comunes
- Escribir código procedural en SQL
- Usar SELECT en bucles de la aplicación

</details>
</details>
