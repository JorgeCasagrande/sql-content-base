# SQL Basics / Fundamentos de SQL

## Índice / Table of Contents

- [1. SELECT Statement / Sentencia SELECT](#1-select-statement--sentencia-select)
- [2. INSERT Statement / Sentencia INSERT](#2-insert-statement--sentencia-insert)
- [3. UPDATE Statement / Sentencia UPDATE](#3-update-statement--sentencia-update)
- [4. DELETE Statement / Sentencia DELETE](#4-delete-statement--sentencia-delete)
- [5. WHERE Clause / Cláusula WHERE](#5-where-clause--cláusula-where)
- [6. ORDER BY / Ordenar resultados](#6-order-by--ordenar-resultados)
- [7. LIMIT / TOP](#7-limit--top)
- [8. DISTINCT](#8-distinct)
- [9. LIKE & Pattern Matching / LIKE y patrones](#9-like--pattern-matching--like-y-patrones)
- [10. BETWEEN, IN, IS NULL](#10-between-in-is-null)
- [11. SQL Comments / Comentarios en SQL](#11-sql-comments--comentarios-en-sql)

---

<details>
<summary><strong>1. SELECT Statement / Sentencia SELECT</strong></summary>

<details>
<summary>🇬🇧 <strong>English</strong></summary>

This module covers the essential SQL statements and syntax every user must know. It is designed to build a strong foundation for all future SQL learning.

---

## 1. SELECT Statement

### Theory
The `SELECT` statement is used to retrieve data from one or more tables in a database. It is the most fundamental SQL command.

- **Purpose:** To query and display data.
- **Syntax:**
  ```sql
  SELECT column1, column2, ...
  FROM table_name;
  ```
- **You can:**
  - Select all columns with `SELECT *`
  - Select specific columns
  - Use expressions and functions in the select list

### Example
Suppose you have a table called `employees`:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

**Select all columns:**
```sql
SELECT * FROM employees;
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

**Select specific columns:**
```sql
SELECT name, salary FROM employees;
```
_Result:_
| name    | salary |
|---------|--------|
| Alice   | 50000  |
| Bob     | 60000  |
| Charlie | 65000  |

**Select with an expression:**
```sql
SELECT name, salary * 1.1 AS increased_salary FROM employees;
```
_Result:_
| name    | increased_salary |
|---------|------------------|
| Alice   | 55000            |
| Bob     | 66000            |
| Charlie | 71500            |

### Best Practices
- Always specify the columns you need (avoid `SELECT *` in production).
- Use aliases (`AS`) for clarity.
- Format your queries for readability.

### Common Mistakes
- Misspelling column or table names.
- Forgetting the `FROM` clause.
- Using `SELECT *` when not necessary.

---

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

Este módulo cubre las sentencias y la sintaxis esenciales de SQL que todo usuario debe conocer. Está diseñado para construir una base sólida para todo aprendizaje futuro de SQL.

---

## 1. Sentencia SELECT

### Teoría
La sentencia `SELECT` se utiliza para recuperar datos de una o más tablas en una base de datos. Es el comando más fundamental de SQL.

- **Propósito:** Consultar y mostrar datos.
- **Sintaxis:**
  ```sql
  SELECT columna1, columna2, ...
  FROM nombre_tabla;
  ```
- **Puedes:**
  - Seleccionar todas las columnas con `SELECT *`
  - Seleccionar columnas específicas
  - Usar expresiones y funciones en la lista de selección

### Ejemplo
Supón que tienes una tabla llamada `employees`:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

**Seleccionar todas las columnas:**
```sql
SELECT * FROM employees;
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

**Seleccionar columnas específicas:**
```sql
SELECT name, salary FROM employees;
```
_Resultado:_
| name    | salary |
|---------|--------|
| Alice   | 50000  |
| Bob     | 60000  |
| Charlie | 65000  |

**Seleccionar con una expresión:**
```sql
SELECT name, salary * 1.1 AS increased_salary FROM employees;
```
_Resultado:_
| name    | increased_salary |
|---------|------------------|
| Alice   | 55000            |
| Bob     | 66000            |
| Charlie | 71500            |

### Buenas Prácticas
- Especifica siempre las columnas que necesitas (evita `SELECT *` en producción).
- Usa alias (`AS`) para mayor claridad.
- Da formato a tus consultas para que sean legibles.

### Errores Comunes
- Escribir mal los nombres de columnas o tablas.
- Olvidar la cláusula `FROM`.
- Usar `SELECT *` cuando no es necesario.

---

</details>

</details>

<details>
<summary><strong>2. INSERT Statement / Sentencia INSERT</strong></summary>

<details>
<summary>🇬🇧 <strong>English</strong></summary>

---

## 2. INSERT Statement

### Theory
The `INSERT` statement is used to add new rows to a table. It is essential for populating your database with data.

- **Purpose:** To add new records to a table.
- **Syntax:**
  ```sql
  INSERT INTO table_name (column1, column2, ...)
  VALUES (value1, value2, ...);
  ```
- You can insert into all columns (if you provide all values) or specify only some columns.

### Example
Suppose you have the same `employees` table:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

**Insert a new employee:**
```sql
INSERT INTO employees (id, name, department, salary)
VALUES (4, 'Diana', 'Finance', 70000);
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |

**Insert with only some columns (others get default or NULL):**
```sql
INSERT INTO employees (name, department)
VALUES ('Eve', 'IT');
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | IT         | NULL   |

### Best Practices
- Always specify the columns you are inserting into.
- Use parameterized queries to avoid SQL injection.
- Check for required fields and constraints.

### Common Mistakes
- Mismatched columns and values count.
- Violating NOT NULL or UNIQUE constraints.
- Forgetting to specify columns (can cause errors if table structure changes).

---

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
La sentencia `INSERT` se utiliza para agregar nuevas filas a una tabla. Es esencial para poblar la base de datos con datos.

- **Propósito:** Agregar nuevos registros a una tabla.
- **Sintaxis:**
  ```sql
  INSERT INTO nombre_tabla (columna1, columna2, ...)
  VALUES (valor1, valor2, ...);
  ```
- Puedes insertar en todas las columnas (si das todos los valores) o solo en algunas.

### Ejemplo
Supón que tienes la misma tabla `employees`:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

**Insertar un nuevo empleado:**
```sql
INSERT INTO employees (id, name, department, salary)
VALUES (4, 'Diana', 'Finance', 70000);
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |

**Insertar solo algunas columnas (otras quedan en NULL o valor por defecto):**
```sql
INSERT INTO employees (name, department)
VALUES ('Eve', 'IT');
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | IT         | NULL   |

### Buenas Prácticas
- Especifica siempre las columnas en las que insertas.
- Usa consultas parametrizadas para evitar inyección SQL.
- Verifica los campos obligatorios y restricciones.

### Errores Comunes
- Cantidad de columnas y valores no coincide.
- Violación de restricciones NOT NULL o UNIQUE.
- Olvidar especificar columnas (puede causar errores si la estructura cambia).

---

</details>

</details>

<details>
<summary><strong>3. UPDATE Statement / Sentencia UPDATE</strong></summary>

<details>
<summary>🇬🇧 <strong>English</strong></summary>

---

## 3. UPDATE Statement

### Theory
The `UPDATE` statement is used to modify existing records in a table. It allows you to change one or more column values for rows that match a condition.

- **Purpose:** To update data in one or more rows.
- **Syntax:**
  ```sql
  UPDATE table_name
  SET column1 = value1, column2 = value2, ...
  WHERE condition;
  ```
- **Important:** Always use a `WHERE` clause to avoid updating all rows.

#### Example
Suppose you have the `employees` table:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

**Update salary for Bob:**
```sql
UPDATE employees
SET salary = 62000
WHERE name = 'Bob';
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

**Update department for all IT employees:**
```sql
UPDATE employees
SET department = 'Tech'
WHERE department = 'IT';
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

#### Best Practices
- Always use a `WHERE` clause unless you intend to update all rows.
- Test your `WHERE` clause with a `SELECT` before running `UPDATE`.
- Backup data before bulk updates.

#### Common Mistakes
- Omitting the `WHERE` clause (updates all rows).
- Using incorrect conditions in `WHERE`.
- Forgetting to commit changes (in transactional databases).

---

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

#### Teoría
La sentencia `UPDATE` se utiliza para modificar registros existentes en una tabla. Permite cambiar uno o más valores de columna para las filas que cumplen una condición.

- **Propósito:** Actualizar datos en una o más filas.
- **Sintaxis:**
  ```sql
  UPDATE nombre_tabla
  SET columna1 = valor1, columna2 = valor2, ...
  WHERE condición;
  ```
- **Importante:** Usa siempre una cláusula `WHERE` para evitar actualizar todas las filas.

#### Ejemplo
Supón que tienes la tabla `employees`:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

**Actualizar el salario de Bob:**
```sql
UPDATE employees
SET salary = 62000
WHERE name = 'Bob';
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

**Actualizar el departamento de todos los empleados de IT:**
```sql
UPDATE employees
SET department = 'Tech'
WHERE department = 'IT';
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

#### Buenas Prácticas
- Usa siempre una cláusula `WHERE` a menos que quieras actualizar todas las filas.
- Prueba tu cláusula `WHERE` con un `SELECT` antes de ejecutar el `UPDATE`.
- Haz respaldo de los datos antes de actualizaciones masivas.

#### Errores Comunes
- Omitir la cláusula `WHERE` (actualiza todas las filas).
- Usar condiciones incorrectas en `WHERE`.
- Olvidar confirmar los cambios (en bases de datos transaccionales).

---

</details>

</details>

<details>
<summary><strong>4. DELETE Statement / Sentencia DELETE</strong></summary>

<details>
<summary>🇬🇧 <strong>English</strong></summary>

---

## 4. DELETE Statement

### Theory
The `DELETE` statement is used to remove one or more rows from a table. It deletes records that match a given condition.

- **Purpose:** To delete data from a table.
- **Syntax:**
  ```sql
  DELETE FROM table_name
  WHERE condition;
  ```
- **Important:** Always use a `WHERE` clause to avoid deleting all rows.

#### Example
Suppose you have the `employees` table:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

**Delete employee Eve:**
```sql
DELETE FROM employees
WHERE name = 'Eve';
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |

**Delete all employees in Tech department:**
```sql
DELETE FROM employees
WHERE department = 'Tech';
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 4  | Diana   | Finance    | 70000  |

#### Best Practices
- Always use a `WHERE` clause unless you intend to delete all rows.
- Test your `WHERE` clause with a `SELECT` before running `DELETE`.
- Backup data before bulk deletions.

#### Common Mistakes
- Omitting the `WHERE` clause (deletes all rows).
- Using incorrect conditions in `WHERE`.
- Forgetting to commit changes (in transactional databases).

---

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

#### Teoría
La sentencia `DELETE` se utiliza para eliminar una o más filas de una tabla. Borra los registros que cumplen una condición dada.

- **Propósito:** Eliminar datos de una tabla.
- **Sintaxis:**
  ```sql
  DELETE FROM nombre_tabla
  WHERE condición;
  ```
- **Importante:** Usa siempre una cláusula `WHERE` para evitar borrar todas las filas.

#### Ejemplo
Supón que tienes la tabla `employees`:

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |
| 5  | Eve     | Tech       | NULL   |

**Eliminar a la empleada Eve:**
```sql
DELETE FROM employees
WHERE name = 'Eve';
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | Tech       | 62000  |
| 3  | Charlie | Tech       | 65000  |
| 4  | Diana   | Finance    | 70000  |

**Eliminar todos los empleados del departamento Tech:**
```sql
DELETE FROM employees
WHERE department = 'Tech';
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 4  | Diana   | Finance    | 70000  |

#### Buenas Prácticas
- Usa siempre una cláusula `WHERE` a menos que quieras borrar todas las filas.
- Prueba tu cláusula `WHERE` con un `SELECT` antes de ejecutar el `DELETE`.
- Haz respaldo de los datos antes de eliminaciones masivas.

#### Errores Comunes
- Omitir la cláusula `WHERE` (borra todas las filas).
- Usar condiciones incorrectas en `WHERE`.
- Olvidar confirmar los cambios (en bases de datos transaccionales).

---

</details>

</details>

<details>
<summary><strong>5. WHERE Clause / Cláusula WHERE</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
The `WHERE` clause filters rows based on a condition. It is used in SELECT, UPDATE, and DELETE statements.

- **Purpose:** To specify which rows to affect.
- **Syntax:**
  ```sql
  SELECT column1, ... FROM table WHERE condition;
  ```
- **Operators:** =, <>, >, <, >=, <=, AND, OR, NOT

#### Example
```sql
SELECT * FROM employees WHERE department = 'IT';
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

#### Best Practices
- Use parentheses to clarify complex conditions.
- Combine conditions with AND/OR carefully.

#### Common Mistakes
- Forgetting quotes for string values.
- Using = instead of <> for NOT EQUAL.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
La cláusula `WHERE` filtra filas según una condición. Se usa en SELECT, UPDATE y DELETE.

- **Propósito:** Especificar qué filas afectar.
- **Sintaxis:**
  ```sql
  SELECT columna1, ... FROM tabla WHERE condición;
  ```
- **Operadores:** =, <>, >, <, >=, <=, AND, OR, NOT

#### Ejemplo
```sql
SELECT * FROM employees WHERE department = 'IT';
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

#### Buenas Prácticas
- Usa paréntesis para condiciones complejas.
- Combina AND/OR con cuidado.

#### Errores Comunes
- Olvidar comillas en valores de texto.
- Usar = en vez de <> para desigualdad.

</details>
</details>

<details>
<summary><strong>6. ORDER BY / Ordenar resultados</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
`ORDER BY` sorts the result set by one or more columns.

- **Syntax:**
  ```sql
  SELECT * FROM employees ORDER BY salary DESC;
  ```
- **Default:** ASC (ascending)

#### Example
```sql
SELECT name, salary FROM employees ORDER BY salary DESC;
```
_Result:_
| name    | salary |
|---------|--------|
| Charlie | 65000  |
| Bob     | 60000  |
| Alice   | 50000  |

#### Best Practices
- Always specify ASC or DESC for clarity.

#### Common Mistakes
- Misspelling column names.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
`ORDER BY` ordena los resultados por una o más columnas.

- **Sintaxis:**
  ```sql
  SELECT * FROM employees ORDER BY salary DESC;
  ```
- **Por defecto:** ASC (ascendente)

#### Ejemplo
```sql
SELECT name, salary FROM employees ORDER BY salary DESC;
```
_Resultado:_
| name    | salary |
|---------|--------|
| Charlie | 65000  |
| Bob     | 60000  |
| Alice   | 50000  |

#### Buenas Prácticas
- Especifica siempre ASC o DESC para mayor claridad.

#### Errores Comunes
- Escribir mal los nombres de columna.

</details>
</details>

<details>
<summary><strong>7. LIMIT / TOP</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
`LIMIT` (MySQL, PostgreSQL) or `TOP` (SQL Server) restricts the number of rows returned.

- **Syntax (MySQL/PostgreSQL):**
  ```sql
  SELECT * FROM employees LIMIT 2;
  ```
- **Syntax (SQL Server):**
  ```sql
  SELECT TOP 2 * FROM employees;
  ```

#### Example
```sql
SELECT * FROM employees LIMIT 2;
```
_Result:_
| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Alice | HR         | 50000  |
| 2  | Bob   | IT         | 60000  |

#### Best Practices
- Use LIMIT/TOP for testing or pagination.

#### Common Mistakes
- Forgetting that LIMIT/TOP order depends on ORDER BY.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
`LIMIT` (MySQL, PostgreSQL) o `TOP` (SQL Server) limita la cantidad de filas devueltas.

- **Sintaxis (MySQL/PostgreSQL):**
  ```sql
  SELECT * FROM employees LIMIT 2;
  ```
- **Sintaxis (SQL Server):**
  ```sql
  SELECT TOP 2 * FROM employees;
  ```

#### Ejemplo
```sql
SELECT * FROM employees LIMIT 2;
```
_Resultado:_
| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Alice | HR         | 50000  |
| 2  | Bob   | IT         | 60000  |

#### Buenas Prácticas
- Usa LIMIT/TOP para pruebas o paginación.

#### Errores Comunes
- Olvidar que el orden depende de ORDER BY.

</details>
</details>

<details>
<summary><strong>8. DISTINCT</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
`DISTINCT` removes duplicate rows from the result set.

- **Syntax:**
  ```sql
  SELECT DISTINCT department FROM employees;
  ```

#### Example
```sql
SELECT DISTINCT department FROM employees;
```
_Result:_
| department |
|------------|
| HR         |
| IT         |
| Finance    |

#### Best Practices
- Use only when needed, as it can impact performance.

#### Common Mistakes
- Using DISTINCT on all columns unintentionally.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
`DISTINCT` elimina filas duplicadas del resultado.

- **Sintaxis:**
  ```sql
  SELECT DISTINCT department FROM employees;
  ```

#### Ejemplo
```sql
SELECT DISTINCT department FROM employees;
```
_Resultado:_
| department |
|------------|
| HR         |
| IT         |
| Finance    |

#### Buenas Prácticas
- Úsalo solo cuando sea necesario, ya que puede afectar el rendimiento.

#### Errores Comunes
- Usar DISTINCT en todas las columnas sin querer.

</details>
</details>

<details>
<summary><strong>9. LIKE & Pattern Matching / LIKE y patrones</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
`LIKE` is used for pattern matching in string columns.

- **Syntax:**
  ```sql
  SELECT * FROM employees WHERE name LIKE 'A%';
  ```
- `%` matches any sequence of characters, `_` matches a single character.

#### Example
```sql
SELECT * FROM employees WHERE name LIKE 'A%';
```
_Result:_
| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Alice | HR         | 50000  |

#### Best Practices
- Use wildcards carefully to avoid slow queries.

#### Common Mistakes
- Forgetting quotes around patterns.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
`LIKE` se usa para buscar patrones en columnas de texto.

- **Sintaxis:**
  ```sql
  SELECT * FROM employees WHERE name LIKE 'A%';
  ```
- `%` representa cualquier secuencia de caracteres, `_` un solo carácter.

#### Ejemplo
```sql
SELECT * FROM employees WHERE name LIKE 'A%';
```
_Resultado:_
| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Alice | HR         | 50000  |

#### Buenas Prácticas
- Usa comodines con cuidado para evitar consultas lentas.

#### Errores Comunes
- Olvidar comillas en los patrones.

</details>
</details>

<details>
<summary><strong>10. BETWEEN, IN, IS NULL</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
- `BETWEEN`: Selects values within a range.
- `IN`: Matches any value in a list.
- `IS NULL`: Checks for NULL values.

#### Example
```sql
SELECT * FROM employees WHERE salary BETWEEN 50000 AND 65000;
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

```sql
SELECT * FROM employees WHERE department IN ('IT', 'Finance');
```
_Result:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |

```sql
SELECT * FROM employees WHERE salary IS NULL;
```
_Result:_
| id | name | department | salary |
|----|------|------------|--------|
| 5  | Eve  | IT         | NULL   |

#### Best Practices
- Use IN for short lists, EXISTS for subqueries.
- Always check for NULLs with IS NULL.

#### Common Mistakes
- Using = NULL instead of IS NULL.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
- `BETWEEN`: Selecciona valores dentro de un rango.
- `IN`: Coincide con cualquier valor de una lista.
- `IS NULL`: Verifica valores nulos.

#### Ejemplo
```sql
SELECT * FROM employees WHERE salary BETWEEN 50000 AND 65000;
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 50000  |
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |

```sql
SELECT * FROM employees WHERE department IN ('IT', 'Finance');
```
_Resultado:_
| id | name    | department | salary |
|----|---------|------------|--------|
| 2  | Bob     | IT         | 60000  |
| 3  | Charlie | IT         | 65000  |
| 4  | Diana   | Finance    | 70000  |

```sql
SELECT * FROM employees WHERE salary IS NULL;
```
_Resultado:_
| id | name | department | salary |
|----|------|------------|--------|
| 5  | Eve  | IT         | NULL   |

#### Buenas Prácticas
- Usa IN para listas cortas, EXISTS para subconsultas.
- Siempre verifica NULL con IS NULL.

#### Errores Comunes
- Usar = NULL en vez de IS NULL.

</details>
</details>

<details>
<summary><strong>11. SQL Comments / Comentarios en SQL</strong></summary>
<details>
<summary>🇬🇧 <strong>English</strong></summary>

### Theory
Comments help document your SQL code. They are ignored by the database engine.

- **Single-line:**
  ```sql
  -- This is a comment
  SELECT * FROM employees; -- Another comment
  ```
- **Multi-line:**
  ```sql
  /*
    This is a
    multi-line comment
  */
  SELECT * FROM employees;
  ```

#### Best Practices
- Use comments to explain complex logic.
- Avoid excessive or outdated comments.

</details>
<details>
<summary>🇪🇸 <strong>Español</strong></summary>

### Teoría
Los comentarios ayudan a documentar tu código SQL. Son ignorados por el motor de la base de datos.

- **Una línea:**
  ```sql
  -- Esto es un comentario
  SELECT * FROM employees; -- Otro comentario
  ```
- **Varias líneas:**
  ```sql
  /*
    Esto es un
    comentario de varias líneas
  */
  SELECT * FROM employees;
  ```

#### Buenas Prácticas
- Usa comentarios para explicar lógica compleja.
- Evita comentarios excesivos o desactualizados.

</details>
</details>

---