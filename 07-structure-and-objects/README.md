# SQL Structure & Objects / Estructura y Objetos en SQL

## Índice / Table of Contents

- [1. CREATE, ALTER, DROP, TRUNCATE](#1-create-alter-drop-truncate)
- [2. Tables, Views, Indexes, Schemas](#2-tables-views-indexes-schemas)
- [3. Constraints (PRIMARY KEY, FOREIGN KEY, etc.)](#3-constraints-primary-key-foreign-key-etc)
- [4. Stored Procedures](#4-stored-procedures)
- [5. User-Defined Functions (UDF)](#5-user-defined-functions-udf)
- [6. Triggers](#6-triggers)
- [7. Security: Users, Roles, Permissions](#7-security-users-roles-permissions)
- [Best Practices](#best-practices)
- [Common Mistakes](#common-mistakes)
- [Anti-patterns & Warnings](#anti-patterns--warnings)
- [Real-world Scenarios / Escenarios del Mundo Real](#real-world-scenarios--escenarios-del-mundo-real)
- [Practice Exercises / Ejercicios Prácticos](#practice-exercises--ejercicios-prácticos)

---

<!-- Cada sección principal será un collapsible, con collapsibles anidados para inglés y español, siguiendo el formato de los otros módulos. -->

<details>
<summary><strong>1. CREATE, ALTER, DROP, TRUNCATE</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
These are Data Definition Language (DDL) commands used to define and modify the structure of database objects.

- **CREATE:** Creates a new table, view, index, schema, etc. Internally, the DBMS allocates metadata and storage structures for the new object. For tables, this includes defining columns, data types, and constraints. Some DBMSs also create system entries for dependencies and permissions. CREATE is a logged operation and can fail if the object already exists (unless IF NOT EXISTS is used).
- **ALTER:** Modifies an existing object (e.g., add/remove columns). ALTER TABLE can add, drop, or modify columns, constraints, or indexes. Internally, the DBMS may need to rewrite the table or update metadata, which can be resource-intensive for large tables. Some ALTER operations are instant in modern DBMS, while others require table locking or data copying.
- **DROP:** Deletes an object from the database. DROP removes the object definition and all associated data and metadata. This operation is usually irreversible and is fully logged. In some DBMS, DROP may also remove dependent objects (like indexes or constraints) automatically.
- **TRUNCATE:** Removes all rows from a table, but keeps its structure. Unlike DELETE, TRUNCATE is usually faster because it deallocates the data pages used by the table, rather than deleting rows one by one. It does not log individual row deletions (minimally logged), and in most DBMS, it cannot be rolled back if not inside a transaction. TRUNCATE does not drop and recreate the table; it simply removes the data efficiently while preserving the table definition, structure, and permissions. Some DBMS reset identity columns with TRUNCATE.

**Syntax Examples:**
```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  department VARCHAR(50)
);

ALTER TABLE employees ADD salary DECIMAL(10,2);

DROP TABLE employees;

TRUNCATE TABLE employees;
```

#### Use Cases
- Creating new tables for data storage
- Modifying table structure as requirements change
- Removing obsolete objects
- Quickly deleting all data from a table

#### Best Practices
- Always backup data before DROP or TRUNCATE
- Use ALTER with caution, especially on large tables
- Use descriptive names for objects

#### Common Mistakes
- Dropping objects accidentally (irreversible!)
- Forgetting that TRUNCATE cannot be rolled back in some DBMS
- Not specifying constraints when creating tables

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Estos son comandos de Lenguaje de Definición de Datos (DDL) usados para definir y modificar la estructura de los objetos de la base de datos.

- **CREATE:** Crea una nueva tabla, vista, índice, esquema, etc. Internamente, el SGBD asigna metadatos y estructuras de almacenamiento para el nuevo objeto. En tablas, esto incluye definir columnas, tipos de datos y restricciones. Algunos SGBD también crean entradas de sistema para dependencias y permisos. CREATE es una operación registrada y puede fallar si el objeto ya existe (a menos que se use IF NOT EXISTS).
- **ALTER:** Modifica un objeto existente (por ejemplo, agregar/eliminar columnas). ALTER TABLE puede agregar, eliminar o modificar columnas, restricciones o índices. Internamente, el SGBD puede necesitar reescribir la tabla o actualizar metadatos, lo que puede ser costoso en tablas grandes. Algunas operaciones ALTER son instantáneas en SGBD modernos, otras requieren bloqueo o copia de datos.
- **DROP:** Elimina un objeto de la base de datos. DROP elimina la definición del objeto y todos los datos y metadatos asociados. Esta operación suele ser irreversible y está completamente registrada. En algunos SGBD, DROP también elimina automáticamente objetos dependientes (índices, restricciones, etc.).
- **TRUNCATE:** Elimina todas las filas de una tabla, pero mantiene su estructura. A diferencia de DELETE, TRUNCATE suele ser mucho más rápido porque libera las páginas de datos usadas por la tabla en lugar de eliminar fila por fila. No registra la eliminación de cada fila (registro mínimo), y en la mayoría de los SGBD, no se puede deshacer si no está dentro de una transacción. TRUNCATE no elimina ni recrea la tabla: simplemente borra los datos de forma eficiente, conservando la definición, estructura y permisos. En algunos SGBD, también reinicia las columnas de identidad.

**Ejemplos de Sintaxis:**
```sql
CREATE TABLE empleados (
  id INT PRIMARY KEY,
  nombre VARCHAR(100),
  departamento VARCHAR(50)
);

ALTER TABLE empleados ADD salario DECIMAL(10,2);

DROP TABLE empleados;

TRUNCATE TABLE empleados;
```

#### Casos de Uso
- Crear nuevas tablas para almacenar datos
- Modificar la estructura de una tabla según cambian los requerimientos
- Eliminar objetos obsoletos
- Borrar rápidamente todos los datos de una tabla

#### Buenas Prácticas
- Haz siempre un respaldo antes de DROP o TRUNCATE
- Usa ALTER con precaución, especialmente en tablas grandes
- Usa nombres descriptivos para los objetos

#### Errores Comunes
- Eliminar objetos accidentalmente (¡irreversible!)
- Olvidar que TRUNCATE no se puede deshacer en algunos SGBD
- No especificar restricciones al crear tablas

</details>
</details>

<details>
<summary><strong>2. Tables, Views, Indexes, Schemas</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
- **Tables:** Store structured data in rows and columns. Internally, the DBMS allocates storage pages and maintains metadata for each table. Table data is organized in data files, and access methods (like heap or clustered index) depend on the DBMS. Table creation also registers the table in system catalogs.
- **Views:** Virtual tables based on queries; do not store data themselves. A view stores only the query definition. When queried, the DBMS dynamically executes the underlying SELECT statement. Some DBMS support materialized views, which store the result set physically and require refresh.
- **Indexes:** Improve query performance by allowing fast data retrieval. Internally, indexes are usually implemented as B-trees or similar structures. Indexes require additional storage and are updated automatically on data changes. Too many indexes can slow down write operations.
- **Schemas:** Logical containers for grouping related database objects. Schemas help organize objects and manage permissions. In most DBMS, schemas are namespaces, and objects in different schemas can have the same name.

**Syntax Examples:**
```sql
CREATE TABLE products (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);

CREATE VIEW active_products AS
SELECT * FROM products WHERE active = 1;

CREATE INDEX idx_name ON products(name);

CREATE SCHEMA sales;
```

#### Use Cases
- Organizing data efficiently
- Creating reusable query logic (views)
- Speeding up searches with indexes
- Structuring large databases with schemas

#### Best Practices
- Use indexes on columns frequently searched or joined
- Keep views simple for maintainability
- Use schemas to separate business domains

#### Common Mistakes
- Over-indexing (can slow down writes)
- Using views for heavy logic (can impact performance)
- Ignoring schema organization in large projects

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
- **Tablas:** Almacenan datos estructurados en filas y columnas. Internamente, el SGBD asigna páginas de almacenamiento y mantiene metadatos para cada tabla. Los datos se organizan en archivos físicos y el método de acceso (heap, índice clusterizado, etc.) depende del motor. La creación de la tabla la registra en los catálogos del sistema.
- **Vistas:** Tablas virtuales basadas en consultas; no almacenan datos por sí mismas. Una vista solo almacena la definición de la consulta. Al consultarla, el SGBD ejecuta dinámicamente el SELECT subyacente. Algunos SGBD soportan vistas materializadas, que almacenan físicamente el resultado y requieren actualización.
- **Índices:** Mejoran el rendimiento de las consultas permitiendo búsquedas rápidas. Internamente, suelen implementarse como B-trees u otras estructuras. Los índices requieren almacenamiento adicional y se actualizan automáticamente con los cambios de datos. Demasiados índices pueden ralentizar las escrituras.
- **Esquemas:** Contenedores lógicos para agrupar objetos relacionados. Los esquemas ayudan a organizar objetos y gestionar permisos. En la mayoría de los SGBD, los esquemas son espacios de nombres y pueden existir objetos con el mismo nombre en diferentes esquemas.

**Ejemplos de Sintaxis:**
```sql
CREATE TABLE productos (
  id INT PRIMARY KEY,
  nombre VARCHAR(100)
);

CREATE VIEW productos_activos AS
SELECT * FROM productos WHERE activo = 1;

CREATE INDEX idx_nombre ON productos(nombre);

CREATE SCHEMA ventas;
```

#### Casos de Uso
- Organizar datos de manera eficiente
- Crear lógica de consulta reutilizable (vistas)
- Acelerar búsquedas con índices
- Estructurar grandes bases de datos con esquemas

#### Buenas Prácticas
- Usa índices en columnas que se buscan o relacionan frecuentemente
- Mantén las vistas simples para facilitar el mantenimiento
- Usa esquemas para separar dominios de negocio

#### Errores Comunes
- Crear demasiados índices (puede ralentizar las escrituras)
- Usar vistas para lógica pesada (puede afectar el rendimiento)
- Ignorar la organización en esquemas en proyectos grandes

</details>
</details>

<details>
<summary><strong>3. Constraints (PRIMARY KEY, FOREIGN KEY, etc.)</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
Constraints enforce rules for data integrity in tables.
- **PRIMARY KEY:** Uniquely identifies each row. Internally, the DBMS creates a unique index on the primary key column(s), which is used to enforce uniqueness and speed up lookups. In some DBMS, the primary key determines the physical order of rows (clustered index).
- **FOREIGN KEY:** Ensures referential integrity between tables. The DBMS checks that values in the foreign key column exist in the referenced table. Foreign key checks can be immediate or deferred, and may lock rows to prevent inconsistent changes.
- **UNIQUE:** Ensures all values in a column are different. Implemented as a unique index, similar to PRIMARY KEY but allows NULLs (depending on DBMS).
- **CHECK:** Restricts values based on a condition. The DBMS evaluates the condition on every insert or update. Some DBMS have limited support for complex CHECK constraints.
- **DEFAULT:** Sets a default value for a column. The DBMS inserts the default value if none is provided.
- **NOT NULL:** Ensures a column cannot have NULL values. Enforced at the storage engine level; attempts to insert NULL will fail.

**Syntax Examples:**
```sql
CREATE TABLE orders (
  id INT PRIMARY KEY,
  customer_id INT,
  amount DECIMAL(10,2) CHECK (amount > 0),
  status VARCHAR(20) DEFAULT 'pending',
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

#### Use Cases
- Enforcing unique identifiers
- Maintaining relationships between tables
- Validating data on insert/update

#### Best Practices
- Always define primary keys
- Use foreign keys to maintain data consistency
- Use CHECK and DEFAULT for business rules

#### Common Mistakes
- Forgetting to define constraints
- Disabling or dropping constraints for convenience
- Overusing NOT NULL or UNIQUE unnecessarily

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Las restricciones aseguran la integridad de los datos en las tablas.
- **PRIMARY KEY:** Identifica de forma única cada fila. Internamente, el SGBD crea un índice único sobre la(s) columna(s) clave primaria, que se usa para garantizar unicidad y acelerar búsquedas. En algunos SGBD, la clave primaria determina el orden físico de las filas (índice clusterizado).
- **FOREIGN KEY:** Garantiza la integridad referencial entre tablas. El SGBD verifica que los valores de la columna foránea existan en la tabla referenciada. Las comprobaciones pueden ser inmediatas o diferidas, y pueden bloquear filas para evitar inconsistencias.
- **UNIQUE:** Asegura que todos los valores de una columna sean diferentes. Se implementa como un índice único, similar a PRIMARY KEY pero permite NULLs (según el SGBD).
- **CHECK:** Restringe valores según una condición. El SGBD evalúa la condición en cada inserción o actualización. Algunos SGBD tienen soporte limitado para CHECK complejos.
- **DEFAULT:** Establece un valor por defecto para una columna. El SGBD inserta el valor por defecto si no se proporciona uno.
- **NOT NULL:** Impide que una columna tenga valores NULL. Se aplica a nivel del motor de almacenamiento; los intentos de insertar NULL fallarán.

**Ejemplo de Sintaxis:**
```sql
CREATE TABLE pedidos (
  id INT PRIMARY KEY,
  cliente_id INT,
  monto DECIMAL(10,2) CHECK (monto > 0),
  estado VARCHAR(20) DEFAULT 'pendiente',
  FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```

#### Casos de Uso
- Garantizar identificadores únicos
- Mantener relaciones entre tablas
- Validar datos en inserciones/actualizaciones

#### Buenas Prácticas
- Define siempre claves primarias
- Usa claves foráneas para mantener la consistencia
- Usa CHECK y DEFAULT para reglas de negocio

#### Errores Comunes
- Olvidar definir restricciones
- Deshabilitar o eliminar restricciones por conveniencia
- Abusar de NOT NULL o UNIQUE sin necesidad

</details>
</details>

<details>
<summary><strong>4. Stored Procedures</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
Stored procedures are precompiled collections of SQL statements and logic, stored in the database and executed as a unit. When created, the DBMS parses and stores the procedure's execution plan for faster execution. Procedures can accept parameters, return results, and include control flow logic (IF, WHILE, etc.). They can be invoked by applications or other procedures. Some DBMS support transaction control within procedures. Procedures are stored in system catalogs and can be granted permissions independently.

**Syntax Example:**
```sql
CREATE PROCEDURE GetEmployeeById (@id INT)
AS
BEGIN
  SELECT * FROM employees WHERE id = @id;
END;
```

#### Use Cases
- Encapsulating business logic
- Reusing complex queries
- Improving performance by reducing network traffic

#### Best Practices
- Use parameters to avoid SQL injection
- Document procedure purpose and parameters
- Keep procedures focused and modular

#### Common Mistakes
- Hardcoding values inside procedures
- Not handling errors or transactions
- Creating overly complex procedures

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Los procedimientos almacenados son colecciones precompiladas de sentencias SQL y lógica, almacenadas en la base de datos y ejecutadas como una unidad. Al crearlos, el SGBD analiza y almacena el plan de ejecución para acelerar su uso. Los procedimientos pueden aceptar parámetros, devolver resultados y contener lógica de control (IF, WHILE, etc.). Pueden ser invocados por aplicaciones u otros procedimientos. Algunos SGBD permiten control transaccional dentro de procedimientos. Se almacenan en los catálogos del sistema y pueden tener permisos independientes.

**Ejemplo de Sintaxis:**
```sql
CREATE PROCEDURE ObtenerEmpleadoPorId (@id INT)
AS
BEGIN
  SELECT * FROM empleados WHERE id = @id;
END;
```

#### Casos de Uso
- Encapsular lógica de negocio
- Reutilizar consultas complejas
- Mejorar el rendimiento reduciendo el tráfico de red

#### Buenas Prácticas
- Usa parámetros para evitar inyección SQL
- Documenta el propósito y los parámetros
- Mantén los procedimientos enfocados y modulares

#### Errores Comunes
- Valores fijos dentro del procedimiento
- No manejar errores o transacciones
- Crear procedimientos demasiado complejos

</details>
</details>

<details>
<summary><strong>5. User-Defined Functions (UDF)</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
User-Defined Functions (UDFs) are routines that return a value or table and can be used in SQL statements. UDFs are stored in the database catalog and can be scalar (return a single value) or table-valued (return a result set). The DBMS compiles and stores the function definition. UDFs are invoked in queries, expressions, or as computed columns. They should be deterministic and free of side effects for best performance. Some DBMS restrict UDFs from modifying data.

**Syntax Example:**
```sql
CREATE FUNCTION GetFullName (@firstName VARCHAR(50), @lastName VARCHAR(50))
RETURNS VARCHAR(101)
AS
BEGIN
  RETURN @firstName + ' ' + @lastName;
END;
```

#### Use Cases
- Reusable calculations or logic
- Simplifying complex queries
- Returning computed columns or tables

#### Best Practices
- Keep functions deterministic (same input, same output)
- Avoid side effects (do not modify data)
- Document input/output clearly

#### Common Mistakes
- Using UDFs for heavy logic (can hurt performance)
- Not handling NULLs properly
- Overusing scalar UDFs in large queries

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Las funciones definidas por el usuario (UDF) son rutinas que devuelven un valor o tabla y pueden usarse en sentencias SQL. Las UDF se almacenan en el catálogo de la base de datos y pueden ser escalares (devuelven un valor) o de tabla (devuelven un conjunto de resultados). El SGBD compila y almacena la definición de la función. Las UDF se invocan en consultas, expresiones o como columnas calculadas. Deben ser deterministas y sin efectos secundarios para mejor rendimiento. Algunos SGBD restringen que las UDF modifiquen datos.

**Ejemplo de Sintaxis:**
```sql
CREATE FUNCTION ObtenerNombreCompleto (@nombre VARCHAR(50), @apellido VARCHAR(50))
RETURNS VARCHAR(101)
AS
BEGIN
  RETURN @nombre + ' ' + @apellido;
END;
```

#### Casos de Uso
- Cálculos o lógica reutilizable
- Simplificar consultas complejas
- Devolver columnas o tablas calculadas

#### Buenas Prácticas
- Mantén las funciones deterministas (mismos datos, mismo resultado)
- Evita efectos secundarios (no modificar datos)
- Documenta claramente entradas y salidas

#### Errores Comunes
- Usar UDFs para lógica pesada (puede afectar el rendimiento)
- No manejar NULLs correctamente
- Abusar de UDFs escalares en consultas grandes

</details>
</details>

<details>
<summary><strong>6. Triggers</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
Triggers are special procedures that automatically execute in response to certain events on a table or view (INSERT, UPDATE, DELETE). Internally, the DBMS registers the trigger and associates it with the specified event. When the event occurs, the trigger code runs automatically, often within the same transaction. Triggers can access OLD and NEW row values, and can modify data or call other procedures. Poorly designed triggers can cause performance issues or recursive calls.

**Syntax Example:**
```sql
CREATE TRIGGER trg_AuditInsert
ON employees
AFTER INSERT
AS
BEGIN
  INSERT INTO audit_log (event, event_date)
  VALUES ('Insert on employees', GETDATE());
END;
```

#### Use Cases
- Auditing changes
- Enforcing business rules
- Cascading actions (e.g., updating related tables)

#### Best Practices
- Keep triggers simple and efficient
- Document trigger purpose and logic
- Avoid recursive or nested triggers

#### Common Mistakes
- Creating triggers that cause performance issues
- Not considering side effects (e.g., infinite loops)
- Using triggers for logic better handled in application code

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
Los triggers son procedimientos especiales que se ejecutan automáticamente en respuesta a ciertos eventos en una tabla o vista (INSERT, UPDATE, DELETE). Internamente, el SGBD registra el trigger y lo asocia al evento especificado. Cuando ocurre el evento, el código del trigger se ejecuta automáticamente, generalmente dentro de la misma transacción. Los triggers pueden acceder a los valores OLD y NEW de las filas, y pueden modificar datos o invocar otros procedimientos. Triggers mal diseñados pueden causar problemas de rendimiento o recursividad.

**Ejemplo de Sintaxis:**
```sql
CREATE TRIGGER trg_AuditoriaInsert
ON empleados
AFTER INSERT
AS
BEGIN
  INSERT INTO log_auditoria (evento, fecha_evento)
  VALUES ('Insert en empleados', GETDATE());
END;
```

#### Casos de Uso
- Auditoría de cambios
- Reglas de negocio
- Acciones en cascada (por ejemplo, actualizar tablas relacionadas)

#### Buenas Prácticas
- Mantén los triggers simples y eficientes
- Documenta el propósito y la lógica
- Evita triggers recursivos o anidados

#### Errores Comunes
- Crear triggers que afectan el rendimiento
- No considerar efectos secundarios (por ejemplo, bucles infinitos)
- Usar triggers para lógica que debería estar en la aplicación

</details>
</details>

<details>
<summary><strong>7. Security: Users, Roles, Permissions</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Theory
Security in SQL databases is managed through users, roles, and permissions. When a user is created, the DBMS stores authentication information and associates the user with roles and permissions. Roles are collections of permissions that simplify management. Permissions are stored in system catalogs and checked on every access. Some DBMS support granular permissions (column-level, schema-level). Security models and syntax can vary between DBMS.

**Syntax Examples:**
```sql
CREATE USER analyst IDENTIFIED BY 'password';
CREATE ROLE reporting;
GRANT SELECT ON employees TO reporting;
GRANT reporting TO analyst;
REVOKE SELECT ON employees FROM reporting;
```

#### Use Cases
- Restricting access to sensitive data
- Assigning permissions by job function
- Auditing and compliance

#### Best Practices
- Grant the least privilege necessary
- Use roles to simplify permission management
- Regularly review and audit permissions

#### Common Mistakes
- Granting excessive permissions
- Not revoking permissions when users change roles
- Using shared accounts

</details>
<details>
<summary><strong>Español</strong></summary>

#### Teoría
La seguridad en bases de datos SQL se gestiona mediante usuarios, roles y permisos. Al crear un usuario, el SGBD almacena la información de autenticación y asocia el usuario a roles y permisos. Los roles agrupan permisos para simplificar la gestión. Los permisos se almacenan en los catálogos del sistema y se verifican en cada acceso. Algunos SGBD permiten permisos granulares (a nivel de columna, esquema, etc.). El modelo y la sintaxis de seguridad varían entre motores.

**Ejemplos de Sintaxis:**
```sql
CREATE USER analista IDENTIFIED BY 'contraseña';
CREATE ROLE reportes;
GRANT SELECT ON empleados TO reportes;
GRANT reportes TO analista;
REVOKE SELECT ON empleados FROM reportes;
```

#### Casos de Uso
- Restringir acceso a datos sensibles
- Asignar permisos según función laboral
- Auditoría y cumplimiento

#### Buenas Prácticas
- Otorga solo los privilegios necesarios
- Usa roles para simplificar la gestión de permisos
- Revisa y audita permisos regularmente

#### Errores Comunes
- Otorgar permisos excesivos
- No revocar permisos cuando los usuarios cambian de rol
- Usar cuentas compartidas

</details>
</details>

<details>
<summary><strong>Best Practices</strong></summary>

<details>
<summary><strong>English</strong></summary>

- Always use meaningful and consistent naming conventions for all objects.
- Document the purpose of tables, procedures, and functions.
- Use version control for database scripts.
- Regularly backup your database, especially before structural changes.
- Apply the principle of least privilege for users and roles.
- Test DDL changes in a development environment before production.
- Normalize tables to avoid redundancy, but denormalize for performance when justified.
- Review and optimize indexes periodically.
- Use transactions for critical changes.

</details>
<details>
<summary><strong>Español</strong></summary>

- Usa siempre convenciones de nombres significativas y consistentes para todos los objetos.
- Documenta el propósito de tablas, procedimientos y funciones.
- Utiliza control de versiones para los scripts de base de datos.
- Realiza respaldos periódicos, especialmente antes de cambios estructurales.
- Aplica el principio de menor privilegio para usuarios y roles.
- Prueba los cambios DDL en un entorno de desarrollo antes de producción.
- Normaliza las tablas para evitar redundancia, pero desnormaliza por rendimiento si es necesario.
- Revisa y optimiza los índices periódicamente.
- Usa transacciones para cambios críticos.

</details>
</details>

<details>
<summary><strong>Common Mistakes</strong></summary>

<details>
<summary><strong>English</strong></summary>

- Dropping or truncating tables without backups.
- Forgetting to define primary or foreign keys.
- Over-indexing or under-indexing tables.
- Using inconsistent naming conventions.
- Granting excessive permissions to users.
- Not documenting changes or object purposes.
- Hardcoding values in procedures or triggers.
- Ignoring error handling in procedures and triggers.
- Not testing DDL changes before production.

</details>
<details>
<summary><strong>Español</strong></summary>

- Eliminar o truncar tablas sin respaldo.
- Olvidar definir claves primarias o foráneas.
- Crear demasiados o muy pocos índices.
- Usar convenciones de nombres inconsistentes.
- Otorgar permisos excesivos a los usuarios.
- No documentar cambios o propósitos de los objetos.
- Valores fijos en procedimientos o triggers.
- Ignorar el manejo de errores en procedimientos y triggers.
- No probar los cambios DDL antes de producción.

</details>
</details>

<details>
<summary><strong>Anti-patterns & Warnings / Anti-patrones y Advertencias</strong></summary>

<details>
<summary><strong>English</strong></summary>

#### Common Anti-patterns
- **Overusing Triggers:** Triggers that implement business logic can make debugging and maintenance difficult. Prefer application logic or stored procedures for complex rules.
- **Monolithic Stored Procedures:** Very large procedures that handle too many responsibilities are hard to test and maintain. Split logic into smaller, reusable procedures.
- **Lack of Version Control:** Not tracking changes to DDL scripts leads to inconsistencies between environments. Use version control for all schema changes.
- **No Error Handling:** Procedures and triggers without error handling can cause silent failures or data corruption.
- **Hardcoding Values:** Avoid hardcoded values in procedures, triggers, or functions. Use parameters and configuration tables.
- **Ignoring Referential Integrity:** Disabling or not defining foreign keys can lead to orphaned records and data inconsistency.
- **Excessive Indexing:** Too many indexes slow down inserts/updates and increase storage usage. Index only what is needed.
- **Granting Broad Permissions:** Avoid using admin or public roles for regular users. Grant only the minimum required permissions.

#### Warnings
- **TRUNCATE is not always recoverable:** In some DBMS, TRUNCATE cannot be rolled back.
- **DROP is irreversible:** Always backup before dropping objects.
- **Triggers can cause performance issues:** Especially with high-frequency tables.
- **Cross-DBMS Syntax Differences:** Always check compatibility when migrating or sharing scripts.

</details>
<details>
<summary><strong>Español</strong></summary>

#### Anti-patrones Comunes
- **Abuso de Triggers:** Los triggers con lógica de negocio dificultan el mantenimiento y la depuración. Prefiere la lógica en la aplicación o procedimientos almacenados para reglas complejas.
- **Procedimientos Monolíticos:** Procedimientos muy grandes y multifunción son difíciles de probar y mantener. Divide la lógica en procedimientos más pequeños y reutilizables.
- **Sin Control de Versiones:** No registrar los cambios de DDL genera inconsistencias entre entornos. Usa control de versiones para todos los cambios de esquema.
- **Sin Manejo de Errores:** Procedimientos y triggers sin manejo de errores pueden causar fallos silenciosos o corrupción de datos.
- **Valores Fijos:** Evita valores fijos en procedimientos, triggers o funciones. Usa parámetros y tablas de configuración.
- **Ignorar Integridad Referencial:** Deshabilitar o no definir claves foráneas puede generar registros huérfanos e inconsistencia.
- **Exceso de Índices:** Demasiados índices ralentizan inserciones/actualizaciones y consumen más espacio. Indexa solo lo necesario.
- **Permisos Demasiado Amplios:** Evita roles de administrador o public para usuarios comunes. Otorga solo los permisos mínimos necesarios.

#### Advertencias
- **TRUNCATE no siempre es recuperable:** En algunos SGBD, TRUNCATE no se puede deshacer.
- **DROP es irreversible:** Haz siempre un respaldo antes de eliminar objetos.
- **Triggers pueden afectar el rendimiento:** Especialmente en tablas de alta frecuencia.
- **Diferencias de Sintaxis entre SGBD:** Verifica compatibilidad al migrar o compartir scripts.

</details>
</details>