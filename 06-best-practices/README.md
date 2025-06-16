# SQL Best Practices / Buenas Prácticas en SQL

## Índice / Table of Contents

- [1. Naming Conventions](#1-naming-conventions)
- [2. Formatting & Style](#2-formatting--style)
- [3. Security](#3-security)
- [4. Anti-patterns](#4-anti-patterns)
- [5. Maintainability](#5-maintainability)
- [6. Documentation & Comments](#6-documentation--comments)
- [7. Testing & Validation](#7-testing--validation)

---

<details>
<summary><strong>1. Naming Conventions</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Consistent naming makes SQL code easier to read, understand, and maintain. Good names reduce ambiguity and help teams collaborate.

- **Tables:** Use singular nouns (e.g., `employee`, `order`).
- **Columns:** Be descriptive but concise (e.g., `created_at`, `total_amount`).
- **Indexes:** Prefix with `idx_` and include table/column (e.g., `idx_employee_name`).
- **Primary/Foreign Keys:** Use `id`, `table_id` (e.g., `employee_id`).
- **Case:** Use snake_case or lowerCamelCase, but be consistent.

#### Example
```sql
CREATE TABLE employee (
  id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  department_id INT
);
CREATE INDEX idx_employee_last_name ON employee(last_name);
```

#### Best Practices
- Avoid reserved words and abbreviations.
- Use clear, meaningful names.
- Be consistent across the database.

#### Common Mistakes
- Inconsistent naming (e.g., `userId` vs. `user_id`).
- Using generic names like `data` or `value`.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Nombrar de forma consistente facilita la lectura, comprensión y mantenimiento del código SQL. Los buenos nombres reducen la ambigüedad y mejoran el trabajo en equipo.

- **Tablas:** Usa sustantivos en singular (ej: `empleado`, `orden`).
- **Columnas:** Sé descriptivo pero conciso (ej: `fecha_creacion`, `monto_total`).
- **Índices:** Prefijo `idx_` e incluye tabla/columna (ej: `idx_empleado_nombre`).
- **Claves primarias/foráneas:** Usa `id`, `tabla_id` (ej: `empleado_id`).
- **Case:** Usa snake_case o lowerCamelCase, pero sé consistente.

#### Ejemplo
```sql
CREATE TABLE empleado (
  id INT PRIMARY KEY,
  nombre VARCHAR(50),
  apellido VARCHAR(50),
  departamento_id INT
);
CREATE INDEX idx_empleado_apellido ON empleado(apellido);
```

#### Buenas Prácticas
- Evita palabras reservadas y abreviaturas.
- Usa nombres claros y significativos.
- Sé consistente en toda la base de datos.

#### Errores Comunes
- Nombres inconsistentes (ej: `usuarioId` vs. `usuario_id`).
- Usar nombres genéricos como `dato` o `valor`.

</details>
</details>

<details>
<summary><strong>2. Formatting & Style</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Consistent formatting improves readability and reduces errors. Well-formatted SQL is easier to debug and review.

- **Indentation:** Use 2-4 spaces per level.
- **Capitalization:** Use uppercase for SQL keywords (SELECT, FROM, WHERE).
- **Line breaks:** Place each clause (SELECT, FROM, WHERE, etc.) on a new line.
- **Alignment:** Align JOIN and ON for clarity.

#### Example
```sql
SELECT e.id, e.first_name, d.name AS department
FROM employee e
JOIN department d ON e.department_id = d.id
WHERE d.name = 'IT'
ORDER BY e.last_name;
```

#### Best Practices
- Use a SQL formatter or linter.
- Keep queries under 80-100 characters per line.
- Comment complex logic.

#### Common Mistakes
- Mixing indentation styles.
- Writing long, unbroken queries.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
El formato consistente mejora la legibilidad y reduce errores. Un SQL bien formateado es más fácil de depurar y revisar.

- **Identación:** Usa 2-4 espacios por nivel.
- **Mayúsculas:** Palabras clave SQL en mayúsculas (SELECT, FROM, WHERE).
- **Saltos de línea:** Cada cláusula (SELECT, FROM, WHERE, etc.) en línea nueva.
- **Alineación:** Alinea JOIN y ON para mayor claridad.

#### Ejemplo
```sql
SELECT e.id, e.nombre, d.nombre AS departamento
FROM empleado e
JOIN departamento d ON e.departamento_id = d.id
WHERE d.nombre = 'IT'
ORDER BY e.apellido;
```

#### Buenas Prácticas
- Usa un formateador o linter de SQL.
- Mantén las líneas por debajo de 80-100 caracteres.
- Comenta la lógica compleja.

#### Errores Comunes
- Mezclar estilos de identación.
- Escribir consultas largas sin saltos de línea.

</details>
</details>

<details>
<summary><strong>3. Security</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Security is critical in SQL to protect data from unauthorized access and attacks like SQL injection.

- **SQL Injection:** Always use parameterized queries or prepared statements.
- **Permissions:** Grant the least privilege necessary.
- **Data exposure:** Avoid exposing sensitive data in queries or logs.

#### Example: Parameterized Query
```sql
-- In application code (e.g., Python)
cursor.execute("SELECT * FROM users WHERE username = ?", (username,))
```

#### Best Practices
- Never concatenate user input into SQL.
- Regularly review user permissions.
- Mask or encrypt sensitive data.

#### Common Mistakes
- Using dynamic SQL with user input.
- Granting excessive privileges.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La seguridad es fundamental en SQL para proteger los datos de accesos no autorizados y ataques como la inyección SQL.

- **Inyección SQL:** Usa siempre consultas parametrizadas o sentencias preparadas.
- **Permisos:** Otorga el menor privilegio necesario.
- **Exposición de datos:** Evita exponer datos sensibles en consultas o logs.

#### Ejemplo: Consulta parametrizada
```sql
-- En código de aplicación (ej: Python)
cursor.execute("SELECT * FROM users WHERE username = ?", (username,))
```

#### Buenas Prácticas
- Nunca concatenes entrada de usuario en SQL.
- Revisa periódicamente los permisos de usuarios.
- Enmascara o cifra datos sensibles.

#### Errores Comunes
- Usar SQL dinámico con entrada de usuario.
- Otorgar privilegios excesivos.

</details>
</details>

<details>
<summary><strong>4. Anti-patterns</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Anti-patterns are common coding practices that lead to problems in maintainability, performance, or security.

- **SELECT *:** Returns unnecessary data, breaks code if schema changes.
- **No WHERE in UPDATE/DELETE:** Can affect all rows by mistake.
- **Overusing subqueries:** Can be less efficient than JOINs.
- **Hardcoding values:** Makes code less flexible.

#### Example
```sql
-- Bad
UPDATE employee SET salary = 0;
-- Good
UPDATE employee SET salary = 0 WHERE id = 123;
```

#### Best Practices
- Always use WHERE in UPDATE/DELETE.
- Prefer explicit column lists.
- Use JOINs instead of subqueries when possible.

#### Common Mistakes
- Forgetting WHERE in data modification queries.
- Relying on SELECT * in production.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Los anti-patrones son prácticas comunes que generan problemas de mantenimiento, rendimiento o seguridad.

- **SELECT *:** Devuelve datos innecesarios, rompe el código si cambia el esquema.
- **Sin WHERE en UPDATE/DELETE:** Puede afectar todas las filas por error.
- **Abusar de subconsultas:** Puede ser menos eficiente que JOINs.
- **Valores hardcodeados:** Hace el código menos flexible.

#### Ejemplo
```sql
-- Mal
UPDATE empleado SET salario = 0;
-- Bien
UPDATE empleado SET salario = 0 WHERE id = 123;
```

#### Buenas Prácticas
- Usa siempre WHERE en UPDATE/DELETE.
- Prefiere listas explícitas de columnas.
- Usa JOINs en vez de subconsultas cuando sea posible.

#### Errores Comunes
- Olvidar WHERE en consultas de modificación de datos.
- Usar SELECT * en producción.

</details>
</details>

<details>
<summary><strong>5. Maintainability</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Maintainable SQL is easy to update, extend, and debug. Good structure and documentation help teams work efficiently.

- **Modular queries:** Use CTEs and views for complex logic.
- **Consistent style:** Follow formatting and naming conventions.
- **Version control:** Store SQL scripts in source control.

#### Best Practices
- Refactor repetitive code into views or CTEs.
- Document assumptions and business rules.
- Use comments for non-obvious logic.

#### Common Mistakes
- Copy-pasting code without refactoring.
- Lack of documentation.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Un SQL mantenible es fácil de actualizar, extender y depurar. Una buena estructura y documentación ayudan al trabajo en equipo.

- **Consultas modulares:** Usa CTEs y vistas para lógica compleja.
- **Estilo consistente:** Sigue convenciones de formato y nombres.
- **Control de versiones:** Guarda scripts SQL en control de versiones.

#### Buenas Prácticas
- Refactoriza código repetitivo en vistas o CTEs.
- Documenta supuestos y reglas de negocio.
- Usa comentarios para lógica no obvia.

#### Errores Comunes
- Copiar y pegar código sin refactorizar.
- Falta de documentación.

</details>
</details>

<details>
<summary><strong>6. Documentation & Comments</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Clear documentation and comments make SQL easier to understand and maintain, especially for complex queries or when working in teams.

- **Inline comments:** Explain non-obvious logic.
- **Block comments:** Document sections or business rules.
- **Schema documentation:** Keep table/column descriptions up to date.

#### Example
```sql
-- Calculate total sales for current year
SELECT SUM(amount) FROM sales WHERE year = 2025;
```

#### Best Practices
- Comment why, not just what.
- Update comments when code changes.

#### Common Mistakes
- Outdated or misleading comments.
- Lack of documentation for complex logic.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La documentación y los comentarios claros facilitan entender y mantener SQL, especialmente en consultas complejas o trabajo en equipo.

- **Comentarios en línea:** Explican lógica no obvia.
- **Comentarios de bloque:** Documentan secciones o reglas de negocio.
- **Documentación de esquema:** Mantén descripciones de tablas/columnas actualizadas.

#### Ejemplo
```sql
-- Calcular ventas totales del año actual
SELECT SUM(monto) FROM ventas WHERE año = 2025;
```

#### Buenas Prácticas
- Comenta el porqué, no solo el qué.
- Actualiza los comentarios cuando cambie el código.

#### Errores Comunes
- Comentarios desactualizados o confusos.
- Falta de documentación en lógica compleja.

</details>
</details>

<details>
<summary><strong>7. Testing & Validation</strong></summary>
<details>
<summary><strong>English</strong></summary>

#### Theory
Testing and validating SQL ensures correctness, prevents regressions, and builds confidence in data quality.

- **Unit tests:** Test individual queries or procedures.
- **Sample data:** Use representative data for testing.
- **Assertions:** Check for expected results and edge cases.

#### Best Practices
- Automate tests for critical queries.
- Validate data after migrations or major changes.
- Use transaction rollbacks in test environments.

#### Common Mistakes
- Not testing edge cases.
- Relying only on manual testing.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Probar y validar SQL asegura la corrección, previene regresiones y da confianza en la calidad de los datos.

- **Pruebas unitarias:** Prueba consultas o procedimientos individuales.
- **Datos de ejemplo:** Usa datos representativos para pruebas.
- **Aserciones:** Verifica resultados esperados y casos límite.

#### Buenas Prácticas
- Automatiza pruebas para consultas críticas.
- Valida datos tras migraciones o cambios importantes.
- Usa rollbacks de transacción en entornos de prueba.

#### Errores Comunes
- No probar casos límite.
- Depender solo de pruebas manuales.

</details>
</details>
