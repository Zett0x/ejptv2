# 🗄️ MySQL

## 📌 Conceptos básicos
- **MySQL** es un **sistema gestor de bases de datos relacional (RDBMS)**, de código abierto, desarrollado y soportado por Oracle.
- Funciona bajo el principio **cliente-servidor**:
  - **Servidor MySQL** → motor que almacena y gestiona los datos.
  - **Clientes MySQL** → realizan consultas SQL para insertar, eliminar, modificar y recuperar datos.
- **MariaDB** es un *fork* de MySQL, creado tras la compra de MySQL por Oracle.

## 🔹 Uso típico
- Muy usado en aplicaciones web dinámicas.
- Comúnmente parte de un stack **LAMP/LEMP** (Linux + Apache/Nginx + MySQL/MariaDB + PHP).
- Ejemplo: **WordPress** guarda entradas, usuarios y contraseñas en MySQL.

## 📂 Estructura
- Datos organizados en **bases de datos** → **tablas** → **columnas** y **filas**.
- Archivos suelen tener extensión **.sql** (ejemplo: *wordpress.sql*).
- Bases de datos internas importantes:
  - **information_schema**: metadatos estándar (ANSI/ISO).
  - **mysql**: tablas de usuarios y permisos.
  - **sys**: vistas y resúmenes de rendimiento.

## ⚠️ Configuración por defecto y riesgos
- Archivo: `/etc/mysql/mysql.conf.d/mysqld.cnf`
- Valores inseguros:
  - `user` y `password` → pueden aparecer en texto plano.
  - `admin_address` → expone la DB por red.
  - `debug` y `sql_warnings` → generan mensajes de error detallados (útiles para ataques).
  - `secure_file_priv` → controla import/export de ficheros.
- Errores comunes:
  - Contraseñas vacías o débiles.
  - Permisos inseguros en el archivo de configuración.
  - Servicios accesibles desde Internet sin restricción.

## 🔎 Footprinting y escaneo
- MySQL suele escuchar en **TCP/3306**.
- Escaneo con Nmap:
  ```bash
  sudo nmap -sV -sC -p3306 --script mysql* <IP>
  ```
  - Detecta versión, credenciales por defecto/vacías y variables de configuración.

## 💻 Interacción con MySQL
- Conexión:
  ```bash
  mysql -u <user> -p<password> -h <IP>
  ```
  *(Ojo: no poner espacio entre `-p` y la contraseña).*

- Comandos útiles:
  ```sql
  show databases;                         -- Lista todas las bases de datos
  use <database>;                         -- Seleccionar una base
  show tables;                            -- Listar tablas de la DB
  show columns from <table>;              -- Ver columnas de una tabla
  select * from <table>;                  -- Ver todos los registros
  select * from <table> where col="x";    -- Buscar valores específicos
  select version();                       -- Mostrar versión de MySQL
  ```

## 🎯 En Pentesting
- Buscar **credenciales débiles** o vacías (`root:` sin contraseña es común).
- Revisar bases sensibles (usuarios, correos, contraseñas).
- Explotar errores de configuración o mensajes de debug.
- Relacionado con ataques de **SQL Injection**.
