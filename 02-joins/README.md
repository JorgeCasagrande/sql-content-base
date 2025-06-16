# SQL Joins / Joins en SQL

## Índice / Table of Contents

- [1. INNER JOIN](#1-inner-join)
- [2. LEFT JOIN](#2-left-join)
- [3. RIGHT JOIN](#3-right-join)
- [4. FULL JOIN](#4-full-join)
- [5. CROSS JOIN](#5-cross-join)
- [6. SELF JOIN](#6-self-join)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)

---

<details>
<summary><strong>1. INNER JOIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
An INNER JOIN returns only the rows where there is a match in both tables, based on the join condition. It is the most common type of join and is used to combine related data from different tables.

- **Purpose:** Retrieve records with matching values in both tables.
- **Syntax:**
  ```sql
  SELECT columns
  FROM table1
  INNER JOIN table2 ON table1.column = table2.column;
  ```
- **How it works:** For each row in `table1`, SQL looks for matching rows in `table2` where the join condition is true. Only those pairs are included in the result.

#### Example
Suppose you have:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```
_Result:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |

#### Visualization
Only rows with matching `department_id` in both tables are included. Departments with no employees (e.g., Marketing) and employees with no department (if any) are excluded.

#### Use Cases
- Combining sales with customer data
- Listing students and their enrolled courses
- Filtering only valid relationships between tables

#### Best Practices
- Always specify the join condition to avoid Cartesian products.
- Use table aliases for clarity, especially with multiple joins.
- Select only the columns you need.

#### Common Mistakes
- Omitting the join condition (results in a Cartesian product).
- Ambiguous column names without table prefix.
- Forgetting to filter NULLs if needed.

#### Advanced Example: Multiple INNER JOINs
```sql
SELECT o.id AS order_id, c.name AS customer, p.name AS product
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
INNER JOIN products p ON o.product_id = p.id;
```

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un INNER JOIN devuelve solo las filas donde hay coincidencia en ambas tablas, según la condición de unión. Es el tipo de join más común y se usa para combinar datos relacionados de diferentes tablas.

- **Propósito:** Recuperar registros con valores coincidentes en ambas tablas.
- **Sintaxis:**
  ```sql
  SELECT columnas
  FROM tabla1
  INNER JOIN tabla2 ON tabla1.columna = tabla2.columna;
  ```
- **Cómo funciona:** Por cada fila en `tabla1`, SQL busca filas coincidentes en `tabla2` donde la condición de unión sea verdadera. Solo esos pares se incluyen en el resultado.

#### Ejemplo
Supón que tienes:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```
_Resultado:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |

#### Visualización
Solo se incluyen las filas con `department_id` coincidente en ambas tablas. Los departamentos sin empleados (por ejemplo, Marketing) y empleados sin departamento (si existieran) se excluyen.

#### Casos de uso
- Combinar ventas con datos de clientes
- Listar estudiantes y sus cursos inscritos
- Filtrar solo relaciones válidas entre tablas

#### Buenas Prácticas
- Especifica siempre la condición de unión para evitar productos cartesianos.
- Usa alias para mayor claridad, especialmente con múltiples joins.
- Selecciona solo las columnas necesarias.

#### Errores Comunes
- Omitir la condición de unión (genera producto cartesiano).
- Nombres de columna ambiguos sin prefijo de tabla.
- Olvidar filtrar NULLs si es necesario.

#### Ejemplo Avanzado: INNER JOIN múltiple
```sql
SELECT o.id AS id_orden, c.name AS cliente, p.name AS producto
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
INNER JOIN products p ON o.product_id = p.id;
```

</details>
</details>

<details>
<summary><strong>2. LEFT JOIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A LEFT JOIN (or LEFT OUTER JOIN) returns all rows from the left table and the matched rows from the right table. If there is no match, the result is NULL on the right side.

- **Purpose:** Retrieve all records from the left table, with matching data from the right table if available.
- **Syntax:**
  ```sql
  SELECT columns
  FROM table1
  LEFT JOIN table2 ON table1.column = table2.column;
  ```
- **How it works:** For each row in `table1`, SQL tries to find a matching row in `table2`. If found, the columns from `table2` are included; if not, those columns are NULL.

#### Example
Suppose you have:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |
| 5  | Eve     | NULL          |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```
_Result:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |
| Eve     | NULL       |

#### Visualization
All employees are listed. If an employee has no department, the department column is NULL. Departments with no employees do not appear.

#### Use Cases
- Listing all users and their last login (even if never logged in)
- Showing all products and their sales (including products with no sales)

#### Best Practices
- Use LEFT JOIN when you want to keep all records from the left table.
- Be aware of NULLs in the result and handle them appropriately.

#### Common Mistakes
- Forgetting that unmatched right-side columns will be NULL.
- Using LEFT JOIN when INNER JOIN is needed (can lead to unexpected NULLs).

#### Advanced Example: LEFT JOIN with Aggregation
```sql
SELECT d.department, COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.department;
```

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un LEFT JOIN (o LEFT OUTER JOIN) devuelve todas las filas de la tabla izquierda y las filas coincidentes de la tabla derecha. Si no hay coincidencia, el resultado es NULL en las columnas de la derecha.

- **Propósito:** Recuperar todos los registros de la tabla izquierda, con datos coincidentes de la derecha si existen.
- **Sintaxis:**
  ```sql
  SELECT columnas
  FROM tabla1
  LEFT JOIN tabla2 ON tabla1.columna = tabla2.columna;
  ```
- **Cómo funciona:** Por cada fila en `tabla1`, SQL busca una fila coincidente en `tabla2`. Si la encuentra, incluye las columnas de `tabla2`; si no, esas columnas son NULL.

#### Ejemplo
Supón que tienes:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |
| 5  | Eve     | NULL          |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```
_Resultado:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |
| Eve     | NULL       |

#### Visualización
Se listan todos los empleados. Si un empleado no tiene departamento, la columna department es NULL. Los departamentos sin empleados no aparecen.

#### Casos de uso
- Listar todos los usuarios y su último acceso (aunque nunca hayan accedido)
- Mostrar todos los productos y sus ventas (incluyendo productos sin ventas)

#### Buenas Prácticas
- Usa LEFT JOIN cuando quieras mantener todos los registros de la tabla izquierda.
- Ten en cuenta los NULLs en el resultado y manéjalos adecuadamente.

#### Errores Comunes
- Olvidar que las columnas de la derecha serán NULL si no hay coincidencia.
- Usar LEFT JOIN cuando se necesita INNER JOIN (puede generar NULLs inesperados).

#### Ejemplo Avanzado: LEFT JOIN con Agregación
```sql
SELECT d.department, COUNT(e.id) AS cantidad_empleados
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.department;
```

</details>
</details>

<details>
<summary><strong>3. RIGHT JOIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A RIGHT JOIN (or RIGHT OUTER JOIN) returns all rows from the right table and the matched rows from the left table. If there is no match, the result is NULL on the left side.

- **Purpose:** Retrieve all records from the right table, with matching data from the left table if available.
- **Syntax:**
  ```sql
  SELECT columns
  FROM table1
  RIGHT JOIN table2 ON table1.column = table2.column;
  ```
- **How it works:** For each row in `table2`, SQL tries to find a matching row in `table1`. If found, the columns from `table1` are included; if not, those columns are NULL.

#### Example
Suppose you have:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |
| 5  | Eve     | NULL          |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```
_Result:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |
| NULL    | Marketing  |

#### Visualization
All departments are listed. If a department has no employees, the name column is NULL. Employees without a department do not appear.

#### Use Cases
- Listing all categories and their products (including categories with no products)
- Showing all events and their attendees (including events with no attendees)

#### Best Practices
- Use RIGHT JOIN when you want to keep all records from the right table.
- Be aware of NULLs in the result and handle them appropriately.

#### Common Mistakes
- Forgetting that unmatched left-side columns will be NULL.
- Using RIGHT JOIN when INNER JOIN or LEFT JOIN is needed (can lead to unexpected NULLs).

#### Advanced Example: RIGHT JOIN with Aggregation
```sql
SELECT d.department, COUNT(e.id) AS employee_count
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id
GROUP BY d.department;
```

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un RIGHT JOIN (o RIGHT OUTER JOIN) devuelve todas las filas de la tabla derecha y las filas coincidentes de la tabla izquierda. Si no hay coincidencia, el resultado es NULL en las columnas de la izquierda.

- **Propósito:** Recuperar todos los registros de la tabla derecha, con datos coincidentes de la izquierda si existen.
- **Sintaxis:**
  ```sql
  SELECT columnas
  FROM tabla1
  RIGHT JOIN tabla2 ON tabla1.columna = tabla2.columna;
  ```
- **Cómo funciona:** Por cada fila en `tabla2`, SQL busca una fila coincidente en `tabla1`. Si la encuentra, incluye las columnas de `tabla1`; si no, esas columnas son NULL.

#### Ejemplo
Supón que tienes:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |
| 5  | Eve     | NULL          |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```
_Resultado:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |
| NULL    | Marketing  |

#### Visualización
Se listan todos los departamentos. Si un departamento no tiene empleados, la columna name es NULL. Los empleados sin departamento no aparecen.

#### Casos de uso
- Listar todas las categorías y sus productos (incluyendo categorías sin productos)
- Mostrar todos los eventos y sus asistentes (incluyendo eventos sin asistentes)

#### Buenas Prácticas
- Usa RIGHT JOIN cuando quieras mantener todos los registros de la tabla derecha.
- Ten en cuenta los NULLs en el resultado y manéjalos adecuadamente.

#### Errores Comunes
- Olvidar que las columnas de la izquierda serán NULL si no hay coincidencia.
- Usar RIGHT JOIN cuando se necesita INNER JOIN o LEFT JOIN (puede generar NULLs inesperados).

#### Ejemplo Avanzado: RIGHT JOIN con Agregación
```sql
SELECT d.department, COUNT(e.id) AS cantidad_empleados
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id
GROUP BY d.department;
```

</details>
</details>

<details>
<summary><strong>4. FULL JOIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A FULL JOIN (or FULL OUTER JOIN) returns all rows when there is a match in one of the tables. Unmatched rows from either table are filled with NULLs.

- **Purpose:** Retrieve all records from both tables, matching where possible, and filling with NULLs where not.
- **Syntax:**
  ```sql
  SELECT columns
  FROM table1
  FULL OUTER JOIN table2 ON table1.column = table2.column;
  ```
- **How it works:** SQL returns all rows from both tables. Where there is a match, the rows are combined. Where there is no match, the missing side is filled with NULLs.

#### Example
Suppose you have:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |
| 5  | Eve     | NULL          |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```
_Result:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |
| Eve     | NULL       |
| NULL    | Marketing  |

#### Visualization
All employees and all departments are listed. If there is no match, the missing side is NULL.

#### Use Cases
- Merging two datasets where you want to keep all records from both
- Reporting on all possible relationships, even if some are missing

#### Best Practices
- Use FULL JOIN when you need a complete view of both tables.
- Handle NULLs in your queries and results.

#### Common Mistakes
- Not handling NULLs in the result.
- Using FULL JOIN when a simpler join would suffice.

#### Advanced Example: FULL JOIN with COALESCE
```sql
SELECT COALESCE(e.name, 'No Employee') AS employee, COALESCE(d.department, 'No Department') AS department
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un FULL JOIN (o FULL OUTER JOIN) devuelve todas las filas cuando hay coincidencia en alguna de las tablas. Las filas no coincidentes de cualquiera de las tablas se rellenan con NULL.

- **Propósito:** Recuperar todos los registros de ambas tablas, emparejando donde sea posible y rellenando con NULL donde no.
- **Sintaxis:**
  ```sql
  SELECT columnas
  FROM tabla1
  FULL OUTER JOIN tabla2 ON tabla1.columna = tabla2.columna;
  ```
- **Cómo funciona:** SQL devuelve todas las filas de ambas tablas. Donde hay coincidencia, las filas se combinan. Donde no hay coincidencia, el lado faltante se rellena con NULL.

#### Ejemplo
Supón que tienes:

**employees**
| id | name    | department_id |
|----|---------|---------------|
| 1  | Alice   | 1             |
| 2  | Bob     | 2             |
| 3  | Charlie | 2             |
| 4  | Diana   | 3             |
| 5  | Eve     | NULL          |

**departments**
| id | department |
|----|------------|
| 1  | HR         |
| 2  | IT         |
| 3  | Finance    |
| 4  | Marketing  |

```sql
SELECT e.name, d.department
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```
_Resultado:_
| name    | department |
|---------|------------|
| Alice   | HR         |
| Bob     | IT         |
| Charlie | IT         |
| Diana   | Finance    |
| Eve     | NULL       |
| NULL    | Marketing  |

#### Visualización
Se listan todos los empleados y todos los departamentos. Si no hay coincidencia, el lado faltante es NULL.

#### Casos de uso
- Unir dos conjuntos de datos donde se quieren conservar todos los registros de ambos
- Reportar todas las relaciones posibles, aunque algunas falten

#### Buenas Prácticas
- Usa FULL JOIN cuando necesites una vista completa de ambas tablas.
- Maneja los NULLs en tus consultas y resultados.

#### Errores Comunes
- No manejar los NULLs en el resultado.
- Usar FULL JOIN cuando un join más simple sería suficiente.

#### Ejemplo Avanzado: FULL JOIN con COALESCE
```sql
SELECT COALESCE(e.name, 'Sin Empleado') AS empleado, COALESCE(d.department, 'Sin Departamento') AS departamento
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

</details>
</details>

<details>
<summary><strong>5. CROSS JOIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A CROSS JOIN returns the Cartesian product of both tables, meaning every row from the first table is combined with every row from the second table.

- **Purpose:** Generate all possible combinations between two tables.
- **Syntax:**
  ```sql
  SELECT * FROM table1 CROSS JOIN table2;
  ```
- **How it works:** If `table1` has 3 rows and `table2` has 4 rows, the result will have 12 rows (3 × 4).

#### Example
Suppose you have:

**colors**
| color  |
|--------|
| Red    |
| Blue   |
| Green  |

**sizes**
| size   |
|--------|
| S      |
| M      |
| L      |
| XL     |

```sql
SELECT c.color, s.size
FROM colors c
CROSS JOIN sizes s;
```
_Result:_
| color | size |
|-------|------|
| Red   | S    |
| Red   | M    |
| Red   | L    |
| Red   | XL   |
| Blue  | S    |
| Blue  | M    |
| Blue  | L    |
| Blue  | XL   |
| Green | S    |
| Green | M    |
| Green | L    |
| Green | XL   |

#### Visualization
All possible combinations of colors and sizes are shown.

#### Use Cases
- Generating all possible product variants
- Creating test data for all combinations

#### Best Practices
- Use CROSS JOIN intentionally; avoid accidental Cartesian products.
- Be cautious with large tables (results can be huge).

#### Common Mistakes
- Forgetting to add a join condition when needed (use INNER JOIN instead).
- Unintentionally creating very large result sets.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un CROSS JOIN devuelve el producto cartesiano de ambas tablas, es decir, cada fila de la primera tabla se combina con cada fila de la segunda tabla.

- **Propósito:** Generar todas las combinaciones posibles entre dos tablas.
- **Sintaxis:**
  ```sql
  SELECT * FROM tabla1 CROSS JOIN tabla2;
  ```
- **Cómo funciona:** Si `tabla1` tiene 3 filas y `tabla2` tiene 4 filas, el resultado tendrá 12 filas (3 × 4).

#### Ejemplo
Supón que tienes:

**colores**
| color  |
|--------|
| Rojo   |
| Azul   |
| Verde  |

**tallas**
| talla  |
|--------|
| S      |
| M      |
| L      |
| XL     |

```sql
SELECT c.color, t.talla
FROM colores c
CROSS JOIN tallas t;
```
_Resultado:_
| color | talla |
|-------|-------|
| Rojo  | S     |
| Rojo  | M     |
| Rojo  | L     |
| Rojo  | XL    |
| Azul  | S     |
| Azul  | M     |
| Azul  | L     |
| Azul  | XL    |
| Verde | S     |
| Verde | M     |
| Verde | L     |
| Verde | XL    |

#### Visualización
Se muestran todas las combinaciones posibles de colores y tallas.

#### Casos de uso
- Generar todas las variantes posibles de un producto
- Crear datos de prueba para todas las combinaciones

#### Buenas Prácticas
- Usa CROSS JOIN de forma intencional; evita productos cartesianos accidentales.
- Ten cuidado con tablas grandes (el resultado puede ser enorme).

#### Errores Comunes
- Olvidar agregar una condición de unión cuando se necesita (usar INNER JOIN en su lugar).
- Crear sin querer conjuntos de resultados muy grandes.

</details>
</details>

<details>
<summary><strong>6. SELF JOIN</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
A SELF JOIN is a regular join where a table is joined with itself. It is useful for comparing rows within the same table.

- **Purpose:** Find relationships within a single table (e.g., employees and their managers).
- **Syntax:**
  ```sql
  SELECT a.column, b.column
  FROM table a
  JOIN table b ON a.related_column = b.related_column;
  ```
- **How it works:** The table is given two different aliases, and the join condition relates rows within the same table.

#### Example
Suppose you have an employees table with a manager_id:

**employees**
| id | name    | manager_id |
|----|---------|------------|
| 1  | Alice   | NULL       |
| 2  | Bob     | 1          |
| 3  | Charlie | 1          |
| 4  | Diana   | 2          |

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.id;
```
_Result:_
| employee | manager |
|----------|---------|
| Alice    | NULL    |
| Bob      | Alice   |
| Charlie  | Alice   |
| Diana    | Bob     |

#### Visualization
Each employee is listed with their manager (if any).

#### Use Cases
- Organizational charts (employees and managers)
- Hierarchical data (categories and subcategories)

#### Best Practices
- Use clear aliases to distinguish the table instances.
- Document the join condition for clarity.

#### Common Mistakes
- Confusing the aliases, leading to incorrect results.
- Forgetting to handle NULLs for top-level rows.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un SELF JOIN es un join normal donde una tabla se une consigo misma. Es útil para comparar filas dentro de la misma tabla.

- **Propósito:** Encontrar relaciones dentro de una sola tabla (por ejemplo, empleados y sus jefes).
- **Sintaxis:**
  ```sql
  SELECT a.columna, b.columna
  FROM tabla a
  JOIN tabla b ON a.columna_relacionada = b.columna_relacionada;
  ```
- **Cómo funciona:** La tabla recibe dos alias diferentes y la condición de unión relaciona filas dentro de la misma tabla.

#### Ejemplo
Supón que tienes una tabla de empleados con manager_id:

**employees**
| id | name    | manager_id |
|----|---------|------------|
| 1  | Alice   | NULL       |
| 2  | Bob     | 1          |
| 3  | Charlie | 1          |
| 4  | Diana   | 2          |

```sql
SELECT e1.name AS empleado, e2.name AS jefe
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.id;
```
_Resultado:_
| empleado | jefe   |
|----------|--------|
| Alice    | NULL   |
| Bob      | Alice  |
| Charlie  | Alice  |
| Diana    | Bob    |

#### Visualización
Cada empleado se muestra con su jefe (si tiene).

#### Casos de uso
- Organigramas (empleados y jefes)
- Datos jerárquicos (categorías y subcategorías)

#### Buenas Prácticas
- Usa alias claros para distinguir las instancias de la tabla.
- Documenta la condición de unión para mayor claridad.

#### Errores Comunes
- Confundir los alias, lo que lleva a resultados incorrectos.
- Olvidar manejar los NULLs para filas de nivel superior.

</details>
</details>

<details>
<summary><strong>Best Practices</strong></summary>

### English
- Always use explicit join syntax (not comma joins).
- Use table aliases for readability.
- Be careful with NULLs in OUTER JOINs.

### Español
- Usa siempre la sintaxis explícita de JOIN (no joins por coma).
- Usa alias para mayor claridad.
- Cuidado con los NULL en OUTER JOINs.

</details>

<details>
<summary><strong>Common Mistakes</strong></summary>

### English
- Forgetting the join condition (causes Cartesian product).
- Ambiguous column names without table prefix.
- Not handling NULLs in OUTER JOINs.

### Español
- Olvidar la condición de join (genera producto cartesiano).
- Nombres de columna ambiguos sin prefijo de tabla.
- No manejar los NULL en OUTER JOINs.

</details>
