# SQL Aggregations / Agregaciones en SQL

## Índice / Table of Contents

- [1. GROUP BY](#1-group-by)
- [2. HAVING](#2-having)
- [3. SUM](#3-sum)
- [4. COUNT](#4-count)
- [5. AVG](#5-avg)
- [6. MIN](#6-min)
- [7. MAX](#7-max)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)

---

<details>
<summary><strong>1. GROUP BY</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `GROUP BY` clause groups rows that have the same values in specified columns into summary rows, like "total salary by department". It is used with aggregate functions (SUM, COUNT, AVG, etc.).

- **Purpose:** Summarize data by categories or groups.
- **Syntax:**
  ```sql
  SELECT column, AGG_FUNCTION(column)
  FROM table
  GROUP BY column;
  ```
- **How it works:** Rows are grouped by the specified column(s), and the aggregate function is applied to each group.

#### Example
Suppose you have:

**employees**
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | IT         | 62000  |

```sql
SELECT department, COUNT(*) AS num_employees
FROM employees
GROUP BY department;
```
_Result:_
| department | num_employees |
|------------|---------------|
| HR         | 1             |
| IT         | 3             |
| Finance    | 1             |

#### Visualization
Each department is listed once, with the number of employees in that department.

#### Use Cases
- Total sales by region
- Number of students per class
- Average score by subject

#### Best Practices
- Always include all non-aggregated columns in the GROUP BY clause.
- Use clear aliases for aggregated columns.

#### Common Mistakes
- Selecting columns not in GROUP BY or not aggregated.
- Forgetting to use aggregate functions with GROUP BY.

#### Advanced Example: Multiple Columns
```sql
SELECT department, salary, COUNT(*)
FROM employees
GROUP BY department, salary;
```

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La cláusula `GROUP BY` agrupa filas que tienen los mismos valores en las columnas especificadas en filas resumen, como "salario total por departamento". Se usa junto con funciones de agregación (SUM, COUNT, AVG, etc.).

- **Propósito:** Resumir datos por categorías o grupos.
- **Sintaxis:**
  ```sql
  SELECT columna, FUNC_AGREGADA(columna)
  FROM tabla
  GROUP BY columna;
  ```
- **Cómo funciona:** Las filas se agrupan por la(s) columna(s) especificada(s) y la función de agregación se aplica a cada grupo.

#### Ejemplo
Supón que tienes:

**employees**
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | IT         | 62000  |

```sql
SELECT department, COUNT(*) AS num_empleados
FROM employees
GROUP BY department;
```
_Resultado:_
| department | num_empleados |
|------------|---------------|
| HR         | 1             |
| IT         | 3             |
| Finance    | 1             |

#### Visualización
Cada departamento aparece una vez, con la cantidad de empleados en ese departamento.

#### Casos de uso
- Ventas totales por región
- Número de estudiantes por clase
- Promedio por materia

#### Buenas Prácticas
- Incluye siempre todas las columnas no agregadas en el GROUP BY.
- Usa alias claros para las columnas agregadas.

#### Errores Comunes
- Seleccionar columnas que no están en el GROUP BY ni son agregadas.
- Olvidar usar funciones de agregación con GROUP BY.

#### Ejemplo Avanzado: Múltiples columnas
```sql
SELECT department, salary, COUNT(*)
FROM employees
GROUP BY department, salary;
```

</details>
</details>

<details>
<summary><strong>2. HAVING</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `HAVING` clause filters records that work on summarized group data. It's like a WHERE clause but for groups, used after the `GROUP BY` clause.

- **Purpose:** Filter groups based on aggregate properties.
- **Syntax:**
  ```sql
  SELECT column, AGG_FUNCTION(column)
  FROM table
  GROUP BY column
  HAVING condition;
  ```
- **How it works:** Groups are formed with `GROUP BY`, then `HAVING` filters these groups.

#### Example
Continuing with the employees table, to find departments with more than 1 employee:

```sql
SELECT department, COUNT(*) AS num_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```
_Result:_
| department | num_employees |
|------------|---------------|
| IT         | 3             |

#### Visualization
Similar to the GROUP BY result, but only showing groups that meet the HAVING condition.

#### Use Cases
- Filter products with total sales above a certain amount.
- Find departments with an average salary below a threshold.

#### Best Practices
- Use `HAVING` for conditions on aggregated data, use `WHERE` for row-level conditions.
- Be mindful of performance, as `HAVING` can be less efficient.

#### Common Mistakes
- Using `HAVING` without `GROUP BY`.
- Confusing `HAVING` with `WHERE`.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La cláusula `HAVING` filtra registros que trabajan con datos de grupo resumidos. Es como una cláusula WHERE pero para grupos, y se usa después de la cláusula `GROUP BY`.

- **Propósito:** Filtrar grupos basados en propiedades agregadas.
- **Sintaxis:**
  ```sql
  SELECT columna, FUNC_AGREGADA(columna)
  FROM tabla
  GROUP BY columna
  HAVING condición;
  ```
- **Cómo funciona:** Se forman grupos con `GROUP BY`, luego `HAVING` filtra estos grupos.

#### Ejemplo
Continuando con la tabla de empleados, para encontrar departamentos con más de 1 empleado:

```sql
SELECT department, COUNT(*) AS num_empleados
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```
_Resultado:_
| department | num_empleados |
|------------|---------------|
| IT         | 3             |

#### Visualización
Similar al resultado de GROUP BY, pero solo mostrando grupos que cumplen con la condición de HAVING.

#### Casos de uso
- Filtrar productos con ventas totales por encima de cierta cantidad.
- Encontrar departamentos con un salario promedio por debajo de un umbral.

#### Buenas Prácticas
- Usa `HAVING` para condiciones sobre datos agregados, usa `WHERE` para condiciones a nivel de fila.
- Ten en cuenta el rendimiento, ya que `HAVING` puede ser menos eficiente.

#### Errores Comunes
- Usar `HAVING` sin `GROUP BY`.
- Confundir `HAVING` con `WHERE`.

</details>
</details>

<details>
<summary><strong>3. SUM</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `SUM` function calculates the total sum of a numeric column. It ignores NULL values.

- **Purpose:** Get the total value of a column.
- **Syntax:**
  ```sql
  SELECT SUM(column)
  FROM table
  WHERE condition;
  ```
- **How it works:** Adds up all values in the column that meet the condition.

#### Example
To find the total salary of all employees:

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```
_Result:_
| total_salary |
|--------------|
| 307000       |

#### Visualization
A single value representing the total sum.

#### Use Cases
- Total sales amount
- Total quantity of items

#### Best Practices
- Use meaningful aliases for the result.
- Ensure the column is numeric.

#### Common Mistakes
- Using SUM on non-numeric columns.
- Forgetting that SUM ignores NULLs.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La función `SUM` calcula la suma total de una columna numérica. Ignora los valores NULL.

- **Propósito:** Obtener el valor total de una columna.
- **Sintaxis:**
  ```sql
  SELECT SUM(columna)
  FROM tabla
  WHERE condición;
  ```
- **Cómo funciona:** Suma todos los valores en la columna que cumplen con la condición.

#### Ejemplo
Para encontrar el salario total de todos los empleados:

```sql
SELECT SUM(salary) AS total_salario
FROM employees;
```
_Resultado:_
| total_salario |
|----------------|
| 307000         |

#### Visualización
Un solo valor que representa la suma total.

#### Casos de uso
- Monto total de ventas
- Cantidad total de artículos

#### Buenas Prácticas
- Usa alias significativos para el resultado.
- Asegúrate de que la columna sea numérica.

#### Errores Comunes
- Usar SUM en columnas no numéricas.
- Olvidar que SUM ignora los NULL.

</details>
</details>

<details>
<summary><strong>4. COUNT</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `COUNT` function returns the number of rows that match a specified criterion. It can count all rows or only those with non-NULL values in a specified column.

- **Purpose:** Count rows or non-NULL values.
- **Syntax:**
  ```sql
  SELECT COUNT(column)
  FROM table
  WHERE condition;
  ```
- **How it works:** Counts the number of rows that meet the condition.

#### Example
To count the total number of employees:

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```
_Result:_
| total_employees |
|-----------------|
| 5               |

To count employees with a specified department:

```sql
SELECT COUNT(department) AS num_departments
FROM employees
WHERE department IS NOT NULL;
```
_Result:_
| num_departments |
|-----------------|
| 5               |

#### Visualization
A single value representing the count.

#### Use Cases
- Count of orders, customers, or products.
- Number of active users.

#### Best Practices
- Use COUNT(*) to count all rows, including duplicates and NULLs.
- Use COUNT(column) to count non-NULL values in a column.

#### Common Mistakes
- Using COUNT on a column with all NULL values and expecting a non-zero result.
- Forgetting that COUNT(column) does not count NULLs.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La función `COUNT` devuelve el número de filas que coinciden con un criterio específico. Puede contar todas las filas o solo aquellas con valores no NULL en una columna específica.

- **Propósito:** Contar filas o valores no NULL.
- **Sintaxis:**
  ```sql
  SELECT COUNT(columna)
  FROM tabla
  WHERE condición;
  ```
- **Cómo funciona:** Cuenta el número de filas que cumplen con la condición.

#### Ejemplo
Para contar el número total de empleados:

```sql
SELECT COUNT(*) AS total_empleados
FROM employees;
```
_Resultado:_
| total_empleados |
|-----------------|
| 5               |

Para contar empleados con un departamento específico:

```sql
SELECT COUNT(department) AS num_departamentos
FROM employees
WHERE department IS NOT NULL;
```
_Resultado:_
| num_departamentos |
|-------------------|
| 5                 |

#### Visualización
Un solo valor que representa el conteo.

#### Casos de uso
- Conteo de pedidos, clientes o productos.
- Número de usuarios activos.

#### Buenas Prácticas
- Usa COUNT(*) para contar todas las filas, incluyendo duplicados y NULLs.
- Usa COUNT(columna) para contar valores no NULL en una columna.

#### Errores Comunes
- Usar COUNT en una columna con todos los valores NULL y esperar un resultado distinto de cero.
- Olvidar que COUNT(columna) no cuenta los NULL.

</details>
</details>

<details>
<summary><strong>5. AVG</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `AVG` function calculates the average value of a numeric column, ignoring NULLs.

- **Purpose:** Get the average value of a column.
- **Syntax:**
  ```sql
  SELECT AVG(column)
  FROM table
  WHERE condition;
  ```
- **How it works:** Adds up all values in the column that meet the condition and divides by the count of those values.

#### Example
To find the average salary of employees:

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```
_Result:_
| average_salary |
|----------------|
| 61400          |

#### Visualization
A single value representing the average.

#### Use Cases
- Average sales, temperatures, or scores.

#### Best Practices
- Ensure the column is numeric.
- Use meaningful aliases.

#### Common Mistakes
- Using AVG on non-numeric columns.
- Forgetting that AVG ignores NULLs.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La función `AVG` calcula el valor promedio de una columna numérica, ignorando los NULL.

- **Propósito:** Obtener el valor promedio de una columna.
- **Sintaxis:**
  ```sql
  SELECT AVG(columna)
  FROM tabla
  WHERE condición;
  ```
- **Cómo funciona:** Suma todos los valores en la columna que cumplen con la condición y divide por el conteo de esos valores.

#### Ejemplo
Para encontrar el salario promedio de los empleados:

```sql
SELECT AVG(salary) AS salario_promedio
FROM employees;
```
_Resultado:_
| salario_promedio |
|------------------|
| 61400            |

#### Visualización
Un solo valor que representa el promedio.

#### Casos de uso
- Promedio de ventas, temperaturas o calificaciones.

#### Buenas Prácticas
- Asegúrate de que la columna sea numérica.
- Usa alias significativos.

#### Errores Comunes
- Usar AVG en columnas no numéricas.
- Olvidar que AVG ignora los NULL.

</details>
</details>

<details>
<summary><strong>6. MIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `MIN` function returns the smallest value in a set of values. It can be used with numeric, date, or string columns.

- **Purpose:** Find the minimum value in a column.
- **Syntax:**
  ```sql
  SELECT MIN(column)
  FROM table
  WHERE condition;
  ```
- **How it works:** Scans the column and returns the smallest value that meets the condition.

#### Example
To find the lowest salary in the employees table:

```sql
SELECT MIN(salary) AS lowest_salary
FROM employees;
```
_Result:_
| lowest_salary |
|---------------|
| 50000         |

#### Visualization
A single value representing the minimum.

#### Use Cases
- Lowest price, earliest date, or smallest quantity.

#### Best Practices
- Ensure the column is appropriate for MIN (e.g., not a text column for lexicographical min).
- Use meaningful aliases.

#### Common Mistakes
- Using MIN on inappropriate data types.
- Forgetting that MIN works on the entire set of values, not just visible rows.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La función `MIN` devuelve el valor más pequeño en un conjunto de valores. Se puede usar con columnas numéricas, de fecha o de texto.

- **Propósito:** Encontrar el valor mínimo en una columna.
- **Sintaxis:**
  ```sql
  SELECT MIN(columna)
  FROM tabla
  WHERE condición;
  ```
- **Cómo funciona:** Escanea la columna y devuelve el valor más pequeño que cumple con la condición.

#### Ejemplo
Para encontrar el salario más bajo en la tabla de empleados:

```sql
SELECT MIN(salary) AS salario_mas_bajo
FROM employees;
```
_Resultado:_
| salario_mas_bajo |
|------------------|
| 50000            |

#### Visualización
Un solo valor que representa el mínimo.

#### Casos de uso
- Precio más bajo, fecha más temprana o cantidad más pequeña.

#### Buenas Prácticas
- Asegúrate de que la columna sea apropiada para MIN (por ejemplo, no una columna de texto para el mínimo lexicográfico).
- Usa alias significativos.

#### Errores Comunes
- Usar MIN en tipos de datos inapropiados.
- Olvidar que MIN opera sobre todo el conjunto de valores, no solo sobre las filas visibles.

</details>
</details>

<details>
<summary><strong>7. MAX</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
The `MAX` function returns the largest value in a set of values. It can be used with numeric, date, or string columns.

- **Purpose:** Find the maximum value in a column.
- **Syntax:**
  ```sql
  SELECT MAX(column)
  FROM table
  WHERE condition;
  ```
- **How it works:** Scans the column and returns the largest value that meets the condition.

#### Example
To find the highest salary in the employees table:

```sql
SELECT MAX(salary) AS highest_salary
FROM employees;
```
_Result:_
| highest_salary |
|----------------|
| 70000          |

#### Visualization
A single value representing the maximum.

#### Use Cases
- Highest price, latest date, or largest quantity.

#### Best Practices
- Ensure the column is appropriate for MAX (e.g., not a text column for lexicographical max).
- Use meaningful aliases.

#### Common Mistakes
- Using MAX on inappropriate data types.
- Forgetting that MAX works on the entire set of values, not just visible rows.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La función `MAX` devuelve el valor más grande en un conjunto de valores. Se puede usar con columnas numéricas, de fecha o de texto.

- **Propósito:** Encontrar el valor máximo en una columna.
- **Sintaxis:**
  ```sql
  SELECT MAX(columna)
  FROM tabla
  WHERE condición;
  ```
- **Cómo funciona:** Escanea la columna y devuelve el valor más grande que cumple con la condición.

#### Ejemplo
Para encontrar el salario más alto en la tabla de empleados:

```sql
SELECT MAX(salary) AS salario_mas_alto
FROM employees;
```
_Resultado:_
| salario_mas_alto |
|------------------|
| 70000            |

#### Visualización
Un solo valor que representa el máximo.

#### Casos de uso
- Precio más alto, fecha más reciente o cantidad más grande.

#### Buenas Prácticas
- Asegúrate de que la columna sea apropiada para MAX (por ejemplo, no una columna de texto para el máximo lexicográfico).
- Usa alias significativos.

#### Errores Comunes
- Usar MAX en tipos de datos inapropiados.
- Olvidar que MAX opera sobre todo el conjunto de valores, no solo sobre las filas visibles.

</details>
</details>

<details>
<summary><strong>Best Practices</strong></summary>

<details>
<summary><strong>English</strong></summary>

- Always specify the columns you need instead of using `SELECT *`.
- Use table aliases to make queries more readable.
- Be explicit about your JOINs (INNER, LEFT, etc.).
- Filter data as early as possible (use WHERE before GROUP BY).
- Keep aggregate functions simple and to the point.
- Document complex queries with comments.

</details>
<details>
<summary><strong>Español</strong></summary>

- Especifica siempre las columnas que necesitas en lugar de usar `SELECT *`.
- Usa alias de tabla para que las consultas sean más legibles.
- Sé explícito sobre tus JOINs (INNER, LEFT, etc.).
- Filtra los datos lo antes posible (usa WHERE antes de GROUP BY).
- Mantén las funciones de agregación simples y directas.
- Documenta consultas complejas con comentarios.

</details>
</details>

<details>
<summary><strong>Common Mistakes</strong></summary>

<details>
<summary><strong>English</strong></summary>

- Forgetting to include all non-aggregated columns in the `GROUP BY` clause.
- Using aggregate functions without `GROUP BY` when needed.
- Confusing `HAVING` with `WHERE`.
- Not using table aliases, leading to ambiguous queries.
- Overusing `SELECT *` in queries.

</details>
<details>
<summary><strong>Español</strong></summary>

- Olvidar incluir todas las columnas no agregadas en la cláusula `GROUP BY`.
- Usar funciones de agregación sin `GROUP BY` cuando es necesario.
- Confundir `HAVING` con `WHERE`.
- No usar alias de tabla, lo que lleva a consultas ambiguas.
- Abusar de `SELECT *` en las consultas.

</details>
</details>

<details>
<summary><strong>English (Advanced)</strong></summary>

- Use indexes on columns filtered before aggregation to improve performance.
- Avoid complex calculations inside aggregate functions; pre-calculate values if possible.
- Prefer CTEs or subqueries to break down complex aggregations for better readability and maintainability.
- Limit aggregations on large datasets without pagination or filters to prevent bottlenecks.
- Review execution plans to identify and optimize slow aggregate queries.
- Be aware of the impact of NULL values on aggregation results.
- When using window functions with aggregations, understand the difference in result sets and use cases.
- Consider database-specific features (e.g., FILTER in PostgreSQL, analytic functions in SQL Server) for advanced aggregations.
- Test queries with realistic data volumes to ensure scalability.

</details>
<details>
<summary><strong>Español (Avanzado)</strong></summary>

- Usa índices en las columnas que filtras antes de agrupar para mejorar el rendimiento.
- Evita cálculos complejos dentro de funciones de agregación; realiza los cálculos previamente si es posible.
- Prefiere CTEs o subconsultas para descomponer agregaciones complejas y mejorar la legibilidad y mantenibilidad.
- Limita el uso de agregaciones en grandes volúmenes de datos sin paginación o filtros para evitar cuellos de botella.
- Revisa los planes de ejecución para identificar y optimizar consultas agregadas lentas.
- Considera el impacto de los valores NULL en los resultados de las agregaciones.
- Si usas funciones de ventana junto con agregaciones, comprende la diferencia en los resultados y los casos de uso.
- Considera características específicas del motor de base de datos (por ejemplo, FILTER en PostgreSQL, funciones analíticas en SQL Server) para agregaciones avanzadas.
- Prueba las consultas con volúmenes de datos realistas para asegurar la escalabilidad.

</details>
