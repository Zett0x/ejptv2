# MSSQL

## Introducción
- **MSSQL (Microsoft SQL Server)**: sistema gestor de bases de datos relacional desarrollado por Microsoft.
- Diferencias clave:
  - Es **código cerrado**.
  - Inicialmente diseñado para **Windows** (aunque hay versiones para Linux/macOS).
- Muy usado en entornos corporativos junto con **.NET Framework**.

---

## Clientes MSSQL
- **SQL Server Management Studio (SSMS)** → principal herramienta gráfica de administración.
- Otros clientes:
  - `mssql-cli`
  - SQL Server PowerShell
  - HeidiSQL
  - SQLPro
  - **Impacket – `mssqlclient.py`** (muy útil en pentesting, viene en Kali/Parrot).

🔎 Localización de `mssqlclient.py`:
```bash
locate mssqlclient
```

---

## Bases de datos por defecto
- **master** → Información global del sistema.
- **model** → Plantilla para crear nuevas bases.
- **msdb** → Usada por SQL Server Agent (tareas programadas, alertas).
- **tempdb** → Objetos temporales.
- **resource** → Base de datos de solo lectura con objetos de sistema.

---

## Configuración por defecto
- Servicio corre como: `NT SERVICE\MSSQLSERVER`.
- Autenticación por defecto: **Windows Authentication** (usa SAM local o Active Directory).
- **Cifrado no forzado** por defecto.
- Riesgo: si se compromete una cuenta de AD con permisos en MSSQL → posible **movimiento lateral** en todo el dominio.

---

## Configuraciones peligrosas
- Conexiones sin cifrado.
- Certificados **self-signed** para TLS (suplantables).
- Uso de **named pipes**.
- Credenciales débiles o por defecto en la cuenta **sa**.
- **`xp_cmdshell` habilitado** → ejecución de comandos del SO desde SQL.

---

## Footprinting MSSQL
### Nmap
Scripts útiles:
```bash
sudo nmap -sV -p 1433 --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER <IP>
```

NOTA : Los scripts en nmap deben de ir separados por "," y sin espacios o dará fallo al encontrarlos.

### Metasploit
```bash
use auxiliary(scanner/mssql/mssql_ping)
set rhosts <IP>
run
```
Muestra información de instancia, versión, puertos y pipes.

---

## Conexión con `mssqlclient.py`
Con credenciales válidas:
```bash
python3 mssqlclient.py usuario:password@IP
# o con autenticación integrada AD
python3 mssqlclient.py usuario@IP -windows-auth
```

### Enumeración inicial
```sql
-- Bases de datos
select name from sys.databases;

-- Tablas
select table_name from information_schema.tables;

-- Ver columnas de una tabla
select column_name, data_type from information_schema.columns where table_name='MiTabla';

-- Mostrar registros de una tabla
select * from MiTabla;

-- Usuarios
select name, type_desc from sys.server_principals;
```

---

## `xp_cmdshell` y ejecución de SO
- Procedimiento extendido que permite ejecutar **comandos del sistema operativo** desde SQL.  
- Ejemplos:
```sql
exec xp_cmdshell 'whoami';
exec xp_cmdshell 'dir C:\';
```

- Si está deshabilitado y tenemos permisos de **sysadmin**:
```sql
exec sp_configure 'show advanced options', 1;
reconfigure;
exec sp_configure 'xp_cmdshell', 1;
reconfigure;
```

- **Riesgo:**  
  - Si el servicio de MSSQL corre como **NT AUTHORITY\SYSTEM**, el atacante obtiene RCE con privilegios máximos.  
  - Desde ahí se pueden extraer credenciales (`mimikatz`, `secretsdump`) y moverse lateralmente por el dominio.

---

## Puntos clave en pentesting
- Buscar credenciales guardadas en SSMS.
- Enumerar usuarios y roles (`sysadmin`, `db_owner`).
- Detectar **bases no por defecto** → posibles datos sensibles.
- Verificar si `xp_cmdshell` está habilitado (o habilitarlo).
- Usar ejecución de comandos para comprometer el sistema operativo.
- Escalar en el dominio si MSSQL usa autenticación de **Active Directory**.

---
