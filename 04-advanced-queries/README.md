# Advanced SQL Queries / Consultas SQL Avanzadas

## Índice / Table of Contents

- [1. Subqueries](#1-subqueries)
- [2. Common Table Expressions (CTEs)](#2-common-table-expressions-ctes)
- [3. Window Functions](#3-window-functions)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)

---

<details>
<summary><strong>1. Subqueries</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A subquery is a query nested inside another SQL query. Subqueries can be used in SELECT, FROM, WHERE, and HAVING clauses to provide dynamic filtering, calculations, or data sets.

- **Purpose:** Break down complex problems, filter results dynamically, or calculate values on the fly.
- **Types:**
  - Scalar subquery (returns a single value)
  - Row subquery (returns a single row)
  - Table subquery (returns multiple rows/columns)
- **Syntax:**
  ```sql
  SELECT column FROM table WHERE column IN (SELECT ... FROM ... WHERE ...);
  ```

#### Example: Subquery in WHERE
Suppose you have:

**employees**
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | IT         | 62000  |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |

Find employees who work in departments with more than one employee:
```sql
SELECT name, department
FROM employees
WHERE department IN (
  SELECT department
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 1
);
```
_Result:_
| name    | department |
|---------|------------|
| Bob     | IT         |
| Charlie | IT         |
| Eve     | IT         |

#### Example: Subquery in SELECT
```sql
SELECT name, salary, (SELECT AVG(salary) FROM employees) AS avg_salary
FROM employees;
```

#### Visualization
Subqueries allow you to use the result of one query as input for another, making queries more flexible and powerful.

#### Use Cases
- Filtering with dynamic lists
- Calculating aggregates for each row
- Correlated subqueries for row-by-row logic

#### Best Practices
- Use subqueries for clarity, but avoid deeply nested subqueries for performance.
- Prefer JOINs over subqueries when possible for better performance.

#### Common Mistakes
- Using subqueries where a JOIN would be more efficient.
- Forgetting that correlated subqueries run once per row (can be slow).

#### Advanced Example: Correlated Subquery
```sql
SELECT name, salary
FROM employees e
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
  WHERE department = e.department
);
```

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Una subconsulta es una consulta anidada dentro de otra consulta SQL. Las subconsultas pueden usarse en SELECT, FROM, WHERE y HAVING para filtrar, calcular o generar conjuntos de datos dinámicos.

- **Propósito:** Descomponer problemas complejos, filtrar resultados dinámicamente o calcular valores en tiempo real.
- **Tipos:**
  - Subconsulta escalar (devuelve un solo valor)
  - Subconsulta de fila (devuelve una sola fila)
  - Subconsulta de tabla (devuelve varias filas/columnas)
- **Sintaxis:**
  ```sql
  SELECT columna FROM tabla WHERE columna IN (SELECT ... FROM ... WHERE ...);
  ```

#### Ejemplo: Subconsulta en WHERE
Supón que tienes:

**employees**
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | IT         | 62000  |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |

Buscar empleados que trabajen en departamentos con más de un empleado:
```sql
SELECT name, department
FROM employees
WHERE department IN (
  SELECT department
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 1
);
```
_Resultado:_
| name    | department |
|---------|------------|
| Bob     | IT         |
| Charlie | IT         |
| Eve     | IT         |

#### Ejemplo: Subconsulta en SELECT
```sql
SELECT name, salary, (SELECT AVG(salary) FROM employees) AS salario_promedio
FROM employees;
```

#### Visualización
Las subconsultas permiten usar el resultado de una consulta como entrada para otra, haciendo las consultas más flexibles y potentes.

#### Casos de uso
- Filtrar con listas dinámicas
- Calcular agregados por fila
- Subconsultas correlacionadas para lógica fila a fila

#### Buenas Prácticas
- Usa subconsultas para claridad, pero evita anidamientos profundos por rendimiento.
- Prefiere JOINs sobre subconsultas cuando sea posible para mejor rendimiento.

#### Errores Comunes
- Usar subconsultas donde un JOIN sería más eficiente.
- Olvidar que las subconsultas correlacionadas se ejecutan por cada fila (puede ser lento).

#### Ejemplo Avanzado: Subconsulta correlacionada
```sql
SELECT name, salary
FROM employees e
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
  WHERE department = e.department
);
```

</details>
</details>

<details>
<summary><strong>2. Common Table Expressions (CTEs)</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A Common Table Expression (CTE) is a named temporary result set that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement. CTEs improve readability and modularity, and can be recursive for hierarchical data.

- **Purpose:** Simplify complex queries, enable recursion, and improve code organization.
- **Syntax:**
  ```sql
  WITH cte_name AS (
    SELECT ...
    FROM ...
    WHERE ...
  )
  SELECT * FROM cte_name;
  ```
- **How it works:** The CTE is defined once and can be referenced multiple times in the main query.

#### Example: Simple CTE
```sql
WITH high_earners AS (
  SELECT name, salary
  FROM employees
  WHERE salary > 60000
)
SELECT * FROM high_earners;
```
_Result:_
| name    | salary |
|---------|--------|
| Charlie | 65000  |
| Diana   | 70000  |
| Eve     | 62000  |

#### Example: Multiple CTEs
```sql
WITH dept_counts AS (
  SELECT department, COUNT(*) AS num_employees
  FROM employees
  GROUP BY department
),
rich_depts AS (
  SELECT department
  FROM employees
  GROUP BY department
  HAVING AVG(salary) > 60000
)
SELECT d.department, d.num_employees
FROM dept_counts d
JOIN rich_depts r ON d.department = r.department;
```

#### Recursive CTE Example
Suppose you have an `employees` table with a `manager_id` column:
```sql
WITH RECURSIVE org_chart AS (
  SELECT id, name, manager_id, 1 AS level
  FROM employees
  WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, oc.level + 1
  FROM employees e
  JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT * FROM org_chart;
```

#### Visualization
CTEs can be used to break down complex logic, and recursive CTEs are powerful for hierarchical data (e.g., org charts, categories).

#### Use Cases
- Modularizing subqueries
- Recursive queries (hierarchies, trees)
- Improving readability of complex SQL

#### Best Practices
- Name CTEs clearly and descriptively.
- Use CTEs to avoid repeating subqueries.
- Limit recursion depth if possible.

#### Common Mistakes
- Forgetting to use the CTE in the main query.
- Creating unnecessary CTEs (can impact performance).
- Infinite recursion in recursive CTEs.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Una Expresión de Tabla Común (CTE) es un conjunto de resultados temporales con nombre que puedes referenciar dentro de una consulta SELECT, INSERT, UPDATE o DELETE. Las CTEs mejoran la legibilidad y modularidad, y pueden ser recursivas para datos jerárquicos.

- **Propósito:** Simplificar consultas complejas, permitir recursividad y mejorar la organización del código.
- **Sintaxis:**
  ```sql
  WITH nombre_cte AS (
    SELECT ...
    FROM ...
    WHERE ...
  )
  SELECT * FROM nombre_cte;
  ```
- **Cómo funciona:** La CTE se define una vez y puede ser referenciada varias veces en la consulta principal.

#### Ejemplo: CTE simple
```sql
WITH altos_sueldos AS (
  SELECT name, salary
  FROM employees
  WHERE salary > 60000
)
SELECT * FROM altos_sueldos;
```
_Resultado:_
| name    | salary |
|---------|--------|
| Charlie | 65000  |
| Diana   | 70000  |
| Eve     | 62000  |

#### Ejemplo: Múltiples CTEs
```sql
WITH conteo_deptos AS (
  SELECT department, COUNT(*) AS num_empleados
  FROM employees
  GROUP BY department
),
deptos_ricos AS (
  SELECT department
  FROM employees
  GROUP BY department
  HAVING AVG(salary) > 60000
)
SELECT d.department, d.num_empleados
FROM conteo_deptos d
JOIN deptos_ricos r ON d.department = r.department;
```

#### Ejemplo de CTE recursiva
Supón que tienes una tabla `employees` con una columna `manager_id`:
```sql
WITH RECURSIVE organigrama AS (
  SELECT id, name, manager_id, 1 AS nivel
  FROM employees
  WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, o.nivel + 1
  FROM employees e
  JOIN organigrama o ON e.manager_id = o.id
)
SELECT * FROM organigrama;
```

#### Visualización
Las CTEs permiten descomponer lógica compleja, y las CTEs recursivas son muy útiles para datos jerárquicos (por ejemplo, organigramas, categorías).

#### Casos de uso
- Modularizar subconsultas
- Consultas recursivas (jerarquías, árboles)
- Mejorar la legibilidad de SQL complejo

#### Buenas Prácticas
- Nombra las CTEs de forma clara y descriptiva.
- Usa CTEs para evitar repetir subconsultas.
- Limita la profundidad de la recursión si es posible.

#### Errores Comunes
- Olvidar usar la CTE en la consulta principal.
- Crear CTEs innecesarias (puede afectar el rendimiento).
- Recursión infinita en CTEs recursivas.

</details>
</details>

<details>
<summary><strong>3. Window Functions</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
Window functions perform calculations across a set of table rows related to the current row, without collapsing the result set. They are powerful for analytics, rankings, and running totals.

- **Purpose:** Calculate values over partitions of data (e.g., running totals, moving averages, rankings).
- **Syntax:**
  ```sql
  SELECT column, AGG_FUNCTION(column) OVER (PARTITION BY col ORDER BY col2) FROM table;
  ```
- **Key clauses:**
  - `PARTITION BY`: Divides the result set into partitions.
  - `ORDER BY`: Defines the order of rows in each partition.

#### Example: ROW_NUMBER
```sql
SELECT name, department, salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```
_Result:_
| name    | department | salary | dept_rank |
|---------|------------|--------|-----------|
| Charlie | IT         | 65000  | 1         |
| Eve     | IT         | 62000  | 2         |
| Bob     | IT         | 60000  | 3         |
| Diana   | Finance    | 70000  | 1         |
| Alice   | HR         | 50000  | 1         |

#### Example: SUM() OVER
```sql
SELECT name, salary,
  SUM(salary) OVER (ORDER BY salary) AS running_total
FROM employees;
```

#### Example: LAG/LEAD
```sql
SELECT name, salary,
  LAG(salary) OVER (ORDER BY salary) AS prev_salary,
  LEAD(salary) OVER (ORDER BY salary) AS next_salary
FROM employees;
```

#### Visualization
Window functions allow you to see both row-level and aggregate data in the same result set, enabling advanced analytics.

#### Use Cases
- Rankings within groups
- Running totals and moving averages
- Comparing current row to previous/next row

#### Best Practices
- Use PARTITION BY and ORDER BY thoughtfully for correct results.
- Name calculated columns clearly.

#### Common Mistakes
- Forgetting to use OVER() with window functions.
- Not partitioning or ordering as needed.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Las funciones de ventana realizan cálculos sobre un conjunto de filas relacionadas con la fila actual, sin colapsar el conjunto de resultados. Son muy potentes para análisis, rankings y totales acumulados.

- **Propósito:** Calcular valores sobre particiones de datos (por ejemplo, totales acumulados, promedios móviles, rankings).
- **Sintaxis:**
  ```sql
  SELECT columna, FUNC_AGREGADA(columna) OVER (PARTITION BY col ORDER BY col2) FROM tabla;
  ```
- **Cláusulas clave:**
  - `PARTITION BY`: Divide el resultado en particiones.
  - `ORDER BY`: Define el orden de las filas en cada partición.

#### Ejemplo: ROW_NUMBER
```sql
SELECT name, department, salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS puesto_depto
FROM employees;
```
_Resultado:_
| name    | department | salary | puesto_depto |
|---------|------------|--------|--------------|
| Charlie | IT         | 65000  | 1            |
| Eve     | IT         | 62000  | 2            |
| Bob     | IT         | 60000  | 3            |
| Diana   | Finance    | 70000  | 1            |
| Alice   | HR         | 50000  | 1            |

#### Ejemplo: SUM() OVER
```sql
SELECT name, salary,
  SUM(salary) OVER (ORDER BY salary) AS total_acumulado
FROM employees;
```

#### Ejemplo: LAG/LEAD
```sql
SELECT name, salary,
  LAG(salary) OVER (ORDER BY salary) AS salario_anterior,
  LEAD(salary) OVER (ORDER BY salary) AS salario_siguiente
FROM employees;
```

#### Visualización
Las funciones de ventana permiten ver datos a nivel de fila y agregados en el mismo resultado, habilitando análisis avanzados.

#### Casos de uso
- Rankings dentro de grupos
- Totales acumulados y promedios móviles
- Comparar la fila actual con la anterior/siguiente

#### Buenas Prácticas
- Usa PARTITION BY y ORDER BY cuidadosamente para obtener resultados correctos.
- Nombra claramente las columnas calculadas.

#### Errores Comunes
- Olvidar usar OVER() con funciones de ventana.
- No particionar u ordenar según sea necesario.

</details>
</details>

<details>
<summary><strong>4. UNION and UNION ALL</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
`UNION` combines the result sets of two or more SELECT statements, removing duplicates. `UNION ALL` includes all rows, including duplicates.

- **Purpose:** Merge results from multiple queries.
- **Syntax:**
  ```sql
  SELECT ... FROM ...
  UNION
  SELECT ... FROM ...;
  ```
- **Requirements:** Each SELECT must have the same number of columns and compatible data types.

#### Example
```sql
SELECT name FROM employees WHERE department = 'IT'
UNION
SELECT name FROM employees WHERE salary > 65000;
```

#### Visualization
`UNION` removes duplicates, `UNION ALL` keeps them.

#### Use Cases
- Merging data from different sources
- Combining results from different filters

#### Best Practices
- Use UNION ALL for performance if duplicates are not a concern.
- Ensure column order and types match.

#### Common Mistakes
- Mismatched columns or types.
- Expecting UNION to keep duplicates.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
`UNION` combina los resultados de dos o más SELECT, eliminando duplicados. `UNION ALL` incluye todas las filas, incluso duplicados.

- **Propósito:** Unir resultados de varias consultas.
- **Sintaxis:**
  ```sql
  SELECT ... FROM ...
  UNION
  SELECT ... FROM ...;
  ```
- **Requisitos:** Cada SELECT debe tener el mismo número de columnas y tipos compatibles.

#### Ejemplo
```sql
SELECT name FROM employees WHERE department = 'IT'
UNION
SELECT name FROM employees WHERE salary > 65000;
```

#### Visualización
`UNION` elimina duplicados, `UNION ALL` los conserva.

#### Casos de uso
- Unir datos de diferentes fuentes
- Combinar resultados de diferentes filtros

#### Buenas Prácticas
- Usa UNION ALL para mejor rendimiento si los duplicados no importan.
- Asegúrate de que el orden y tipo de columnas coincidan.

#### Errores Comunes
- Columnas o tipos incompatibles.
- Esperar que UNION conserve duplicados.

</details>
</details>

<details>
<summary><strong>5. EXISTS and NOT EXISTS</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
`EXISTS` and `NOT EXISTS` test for the existence of rows in a subquery. They are efficient for checking presence or absence of related data.

- **Purpose:** Filter results based on the existence of related rows.
- **Syntax:**
  ```sql
  SELECT column FROM table WHERE EXISTS (SELECT 1 FROM ... WHERE ...);
  ```

#### Example
Find departments with at least one employee:
```sql
SELECT department
FROM departments d
WHERE EXISTS (
  SELECT 1 FROM employees e WHERE e.department = d.department
);
```

#### Visualization
`EXISTS` returns TRUE if the subquery returns any rows.

#### Use Cases
- Filtering parent rows with/without children
- Efficient existence checks

#### Best Practices
- Use EXISTS for correlated subqueries.
- Prefer EXISTS over IN for large subqueries.

#### Common Mistakes
- Using EXISTS with non-correlated subqueries (less efficient).
- Confusing EXISTS with IN (different semantics).

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
`EXISTS` y `NOT EXISTS` prueban la existencia de filas en una subconsulta. Son eficientes para comprobar la presencia o ausencia de datos relacionados.

- **Propósito:** Filtrar resultados según la existencia de filas relacionadas.
- **Sintaxis:**
  ```sql
  SELECT columna FROM tabla WHERE EXISTS (SELECT 1 FROM ... WHERE ...);
  ```

#### Ejemplo
Buscar departamentos con al menos un empleado:
```sql
SELECT department
FROM departments d
WHERE EXISTS (
  SELECT 1 FROM employees e WHERE e.department = d.department
);
```

#### Visualización
`EXISTS` devuelve TRUE si la subconsulta retorna alguna fila.

#### Casos de uso
- Filtrar filas padre con/sin hijos
- Comprobaciones de existencia eficientes

#### Buenas Prácticas
- Usa EXISTS para subconsultas correlacionadas.
- Prefiere EXISTS sobre IN para subconsultas grandes.

#### Errores Comunes
- Usar EXISTS con subconsultas no correlacionadas (menos eficiente).
- Confundir EXISTS con IN (tienen semánticas diferentes).

</details>
</details>

<details>
<summary><strong>6. WITH ROLLUP and WITH CUBE</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
`WITH ROLLUP` and `WITH CUBE` are extensions to GROUP BY that produce subtotals and grand totals.

- **Purpose:** Generate summary rows for reporting.
- **Syntax:**
  ```sql
  SELECT department, SUM(salary)
  FROM employees
  GROUP BY department WITH ROLLUP;
  ```

#### Example
```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY department WITH ROLLUP;
```

#### Visualization
Adds extra rows for subtotals and totals.

#### Use Cases
- Financial reports
- Sales summaries

#### Best Practices
- Use only when you need subtotals/totals.
- Label NULLs in result as "Total" or "Subtotal".

#### Common Mistakes
- Misinterpreting NULLs in subtotal/total rows.
- Using with incompatible SQL dialects.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
`WITH ROLLUP` y `WITH CUBE` son extensiones de GROUP BY que generan subtotales y totales generales.

- **Propósito:** Generar filas resumen para reportes.
- **Sintaxis:**
  ```sql
  SELECT department, SUM(salary)
  FROM employees
  GROUP BY department WITH ROLLUP;
  ```

#### Ejemplo
```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY department WITH ROLLUP;
```

#### Visualización
Agrega filas extra para subtotales y totales.

#### Casos de uso
- Reportes financieros
- Resúmenes de ventas

#### Buenas Prácticas
- Úsalo solo cuando necesites subtotales/totales.
- Etiqueta los NULLs en el resultado como "Total" o "Subtotal".

#### Errores Comunes
- Malinterpretar los NULLs en filas de subtotal/total.
- Usar en dialectos SQL que no lo soportan.

</details>
</details>

<details>
<summary><strong>Best Practices</strong></summary>

<details>
<summary><strong>English</strong></summary>

- **Clarity over Cleverness:** Write queries that are easy to understand rather than trying to be overly clever with complex syntax.
- **Use Explicit JOINs:** Always use explicit JOIN syntax (INNER JOIN, LEFT JOIN, etc.) instead of commas in the FROM clause for clarity.
- **Alias Wisely:** Use table and column aliases to make your queries more readable, especially when using multiple tables or complex expressions.
- **Limit SELECT *:** Avoid using SELECT * in production queries. Be explicit about the columns you need.
- **Comment Your Code:** Use comments to explain the purpose of complex queries or non-obvious logic.

</details>
<details>
<summary><strong>Español</strong></summary>

- **Claridad sobre Ingenio:** Escribe consultas que sean fáciles de entender en lugar de intentar ser demasiado ingenioso con una sintaxis compleja.
- **Usa JOINs Explícitos:** Siempre usa la sintaxis de JOIN explícita (INNER JOIN, LEFT JOIN, etc.) en lugar de comas en la cláusula FROM para mayor claridad.
- **Alias con Sabiduría:** Usa alias para tablas y columnas para que tus consultas sean más legibles, especialmente al usar múltiples tablas o expresiones complejas.
- **Limita SELECT *:** Evita usar SELECT * en consultas de producción. Sé explícito sobre las columnas que necesitas.
- **Comenta tu Código:** Usa comentarios para explicar el propósito de consultas complejas o lógica no obvia.

</details>
</details>

<details>
<summary><strong>Common Mistakes</strong></summary>

<details>
<summary><strong>English</strong></summary>

- **Ignoring NULLs:** Not considering how NULL values affect query results, especially in JOIN and WHERE conditions.
- **Mismatched Data Types:** Failing to ensure that data types match in comparisons, which can lead to unexpected results or errors.
- **Overusing DISTINCT:** Using DISTINCT unnecessarily can lead to performance issues; it's often better to address the root cause of duplicate rows.
- **Improper Use of Aggregates:** Misunderstanding how aggregate functions work, particularly with GROUP BY and HAVING clauses.
- **Neglecting Indexes:** Not using indexes effectively can result in slow query performance, especially on large tables.

</details>
<details>
<summary><strong>Español</strong></summary>

- **Ignorar NULLs:** No considerar cómo los valores NULL afectan los resultados de las consultas, especialmente en condiciones JOIN y WHERE.
- **Tipos de Datos Desajustados:** No asegurar que los tipos de datos coincidan en las comparaciones, lo que puede llevar a resultados inesperados o errores.
- **Uso Excessivo de DISTINCT:** Usar DISTINCT innecesariamente puede llevar a problemas de rendimiento; a menudo es mejor abordar la causa raíz de las filas duplicadas.
- **Uso Incorrecto de Agregados:** Malinterpretar cómo funcionan las funciones de agregado, particularmente con las cláusulas GROUP BY y HAVING.
- **Descuidar Índices:** No usar índices de manera efectiva puede resultar en un rendimiento lento de las consultas, especialmente en tablas grandes.

</details>
</details>
