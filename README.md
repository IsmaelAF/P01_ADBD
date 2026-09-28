# P01_ADBD
Conceptos fundamentales de PostgreSQL

## 1. Creación de la base de datos "biblioteca"
##### Comando
```sql
CREATE DATABASE biblioteca;
```
###### Salida 
```text
CREATE DATABASE
```

## 2. Creación de usuarios:
a. Crear dos usuario:
- admin_biblio con permisos de administrador sobre la base de datos.
```sql
CREATE USER admin_biblio WITH PASSWORD 'admin1234';
GRANT ALL PRIVILEGES ON DATABASE biblioteca TO admin_biblio;
```
- usuario_biblio.
```sql
CREATE USER usuario_biblio WITH PASSWORD 'usuario1234';
```
b. Crear un rol llamado lectores con permisos únicamente de consulta sobre las tablas de la base de datos.
```sql
CREATE ROLE lectores;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lectores;
```
c. Asignar el usuario usuario_biblio a este rol.
```sql
GRANT lectores TO usuario_biblio;
```
d. Consultar las tablas del sistema para listar todos los usuarios creados (pg_roles).
```sql
SELECT rolname FROM pg_roles;
```
###### Salida:
```text
           rolname           
-----------------------------
 pg_database_owner
 pg_read_all_data
 pg_write_all_data
 pg_monitor
 pg_read_all_settings
 pg_read_all_stats
 pg_stat_scan_tables
 pg_read_server_files
 pg_write_server_files
 pg_execute_server_program
 pg_signal_backend
 pg_checkpoint
 pg_use_reserved_connections
 pg_create_subscription
 postgres
 mydb_admin
 admin_biblio
 usuario_biblio
 lectores
(19 rows)
```
e. Cambiar la contraseña del usuario usuario_biblio.
```sql
ALTER USER usuario_biblio WITH PASSWORD '1234';
```
f. Configurar permisos de tal forma que el usuario usuario_biblio no pueda eliminar registros en ninguna tabla.
```sql
REVOKE DELETE ON ALL TABLES IN SCHEMA public FROM usuario_biblio;
```
## 3. Creación de tablas.
a. Crear las siguientes tablas con sus respectivas claves primarias y foráneas.
  i. autores:
  ```sql
  CREATE TABLE autores (
    id_autor SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50)
  );
  ```
  ii. libros:
  ```sql
  CREATE TABLE libros (
    id_libro SERIAL PRIMARY KEY,
    titulo VARCHAR(200) NOT NULL,
    ano_publicacion INT,
    id_autor INT,
    FOREIGN KEY (id_autor)
      REFERENCES autores(id_autor)
      ON DELETE CASCADE
  );
  ```
iii. prestamos:
  ```sql
  CREATE TABLE prestamos (
    id_prestamo SERIAL PRIMARY KEY,
    id_libro INT,
    fecha_prestamo DATE NOT NULL,
    fecha_devolucion DATE,
    usuario_prestatario VARCHAR(100),
    FOREIGN KEY (id_libro)
      REFERENCES libros(id_libro)
      ON DELETE CASCADE
  );
  ```
## 4. Inserción de datos.
Insertar al menos 5 autores, 8 libros y 5 préstamos de ejemplo.
- 5 autores:
  ```sql
  INSERT INTO autores (nombre, nacionalidad) VALUES
  ('Ismael Acosta Febles', 'Español'),
  ('Alejandro David Castro Afonso', 'Español'),
  ('Gabriel García Márquez', 'Colombia'),
  ('Isabel Allende', 'Chile'),
  ('Mario Vargas Llosa', 'Peru');
  ```
  Comprobación: 
  ```text
   select * from autores;
   id_autor |            nombre             | nacionalidad 
  ----------+-------------------------------+--------------
        1 | Ismael Acosta Febles          | Español
        2 | Alejandro David Castro Afonso | Español
        3 | Gabriel García Márquez        | Colombia
        4 | Isabel Allende                | Chile
        5 | Mario Vargas Llosa            | Peru
  (5 rows)
  ```

- 8 libros:
  ```sql
  INSERT INTO libros (titulo, ano_publicacion, id_autor) VALUES
  ('Base de datos', 2026, 1),
  ('ESIT', 2025, 2),
  ('Cronica de una muerte anunciada', 2001, 3),
  ('El coronel no tiene quien le escriba', 2002, 3),
  ('Eva Luna', 2003, 4),
  ('La casa de los espiritus', 2004, 4),
  ('La ciudad y los perros', 2005, 5),
  ('La guerra del fin del mundo', 2006, 5);
  ```
  Comprobación: 
  ```text
   select * from libros;
   id_libro |                titulo                | ano_publicacion | id_autor 
  ----------+--------------------------------------+-----------------+----------
        1 | Base de datos                        |            2026 |        1
        2 | ESIT                                 |            2025 |        2
        3 | Cronica de una muerte anunciada      |            2001 |        3
        4 | El coronel no tiene quien le escriba |            2002 |        3
        5 | Eva Luna                             |            2003 |        4
        6 | La casa de los espiritus             |            2004 |        4
        7 | La ciudad y los perros               |            2005 |        5
        8 | La guerra del fin del mundo          |            2006 |        5
  (8 rows)
  ```
- 5 préstamos:
  ```sql
  
  ```
  
