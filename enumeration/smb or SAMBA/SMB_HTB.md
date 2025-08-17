# 📡 SMB y Samba

## 🔹 SMB (Server Message Block)
- Protocolo cliente-servidor que permite:
  - Compartir **archivos y directorios**.  
  - Acceder a **impresoras, routers y otros recursos** en red.  
  - Comunicación entre procesos en red.  
- Usado principalmente en **Windows**, pero también soportado en **Linux/Unix** mediante **Samba**.  
- Se establece sobre **TCP** (handshake de 3 vías).  

### 🏷️ Versiones de SMB
| Versión | Sistemas | Características |
|---------|----------|-----------------|
| **CIFS (SMB1)** | Windows NT 4.0 | Funciona sobre NetBIOS (puertos 137-139). |
| **SMB 1.0** | Windows 2000 | Conexión directa vía TCP. |
| **SMB 2.0** | Vista / 2008 | Mejor rendimiento, caching, signing mejorado. |
| **SMB 2.1** | 7 / 2008 R2 | Mecanismos de bloqueo. |
| **SMB 3.0** | 8 / 2012 | Multicanal, cifrado extremo a extremo. |
| **SMB 3.1.1** | 10 / 2016 | Integridad, AES-128. |

⚠️ **SMB1 (CIFS)** está obsoleto y es inseguro, pero aún puede encontrarse en entornos legacy.  

---

## 🔹 Samba
- Implementación libre de SMB en Linux/Unix.  
- Permite interoperabilidad con Windows (SMB/CIFS).  
- Con Samba 3 → puede ser miembro de un **dominio AD**.  
- Con Samba 4 → puede actuar como **Controlador de Dominio AD**.  

### Configuración básica (`/etc/samba/smb.conf`)
- `[global]` → ajustes generales.  
- `[sharename]` → configuración de cada recurso compartido.  

Parámetros importantes:
- `workgroup` → grupo de trabajo o dominio.  
- `path` → directorio compartido.  
- `browseable = yes/no` → listar el recurso en el explorador.  
- `guest ok = yes/no` → acceso sin credenciales.  
- `read only = yes/no` → permisos de escritura.  
- `create mask` / `directory mask` → permisos por defecto.  

### 🚩 Configuraciones peligrosas
- `guest ok = yes` → acceso sin password.  
- `browseable = yes` → los shares son visibles a cualquiera.  
- `writable = yes` + `create mask = 0777` → escritura sin restricciones.  
- `enable privileges = yes` → respeta privilegios de SID (riesgo si mal configurado).  

---

## 🔎 Enumeración SMB

### 📂 smbclient
- Listar shares (sesión nula):  
  ```bash
  smbclient -N -L //IP
  ```
- Conectar a un share:  
  ```bash
  smbclient //IP/share -N
  ```
- Comandos dentro del cliente:  
  - `ls` → listar contenido.  
  - `get file.txt` → descargar.  
  - `put file.txt` → subir.  
  - `!ls`, `!cat` → ejecutar comandos locales sin salir.  

---

## 🔎 rpcclient – Detalle

### 📌 ¿Qué es?
- Herramienta de Samba que permite ejecutar llamadas **MS-RPC (Microsoft Remote Procedure Call)** contra un servidor Windows o Samba.  
- RPC es el mecanismo que Windows usa para comunicación con servicios internos (usuarios, grupos, shares, políticas de dominio).  
- Permite **consultar información sensible**, muchas veces con **null sessions** (sin credenciales).  

### 📌 ¿Para qué sirve?
Con `rpcclient` podemos:  

#### 🔹 Enumeración de Servidor
- `srvinfo` → versión del SO, nombre de host, tipo de servidor.  
- `enumdomains` → lista de dominios.  
- `querydominfo` → info del dominio (usuarios totales, grupos, rol del servidor).  

#### 🔹 Enumeración de Shares
- `netshareenumall` → lista todos los recursos compartidos.  
- `netsharegetinfo <share>` → detalles de un share (ruta completa, permisos).  

#### 🔹 Enumeración de Usuarios y Grupos
- `enumdomusers` → lista de usuarios de dominio.  
- `queryuser <RID>` → información detallada de un usuario (nombre, perfil, fechas de password).  
- `querygroup <RID>` → información de un grupo específico.  
- `enumdomgroups` → lista de grupos del dominio.  

#### 🔹 Políticas de Dominio
- `querydompwinfo` → información de políticas de contraseña (longitud mínima, expiración, complejidad).  
- `enumtrustdom` → enumera dominios de confianza.  

#### 🔹 Sesiones activas
- `enumdomsessions` → ver sesiones SMB activas en el servidor.  

#### 🔹 Brute Force de RIDs
Permite identificar usuarios aunque no sepamos sus nombres:  
```bash
for i in $(seq 500 1100); do
  rpcclient -N -U "" IP -c "queryuser 0x$(printf '%x
' $i)"   | grep "User Name\|user_rid\|group_rid"
done
```

### 📌 Ejemplos prácticos
```bash
# Conexión anónima (null session)
rpcclient -N -U "" IP

# Info del servidor
rpcclient $> srvinfo

# Shares disponibles (muestra la ruta local absoluta donde se alojan esos recursos compartidos, si el servidor es un windows y porne C:\home\user, en realidad estára en /home/user)
rpcclient $> netshareenumall

# Info de un share concreto
rpcclient $> netsharegetinfo notes

# Enumerar usuarios
rpcclient $> enumdomusers

# Info de un usuario concreto
rpcclient $> queryuser 0x3e8

# Info de un grupo
rpcclient $> querygroup 0x201
```

### 📌 Importancia
- Descubre **usuarios, grupos, shares, políticas** sin credenciales.  
- Más **detallado** que un escaneo con Nmap.  
- Es una de las **herramientas estándar** en pentesting SMB junto a SMBMap, CrackMapExec y enum4linux-ng.  

---

## 🧰 Otras herramientas de enumeración
- **Nmap**:  
  ```bash
  sudo nmap -sV -sC -p139,445 IP
  ```
  Scripts útiles: `smb-os-discovery`, `smb-enum-users`, `smb-enum-shares`, `smb-vuln*`.

- **Impacket** → `samrdump.py IP` (usuarios/grupos).  

- **SMBMap** → enumera shares y permisos:  
  ```bash
  smbmap -H IP
  ```

- **CrackMapExec** → enumeración avanzada:  
  ```bash
  crackmapexec smb IP --shares -u '' -p ''
  ```

- **Enum4linux-ng** → automatiza muchas consultas de SMB:  
  ```bash
  ./enum4linux-ng.py IP -A
  ```

---

## ⚠️ Riesgos comunes
- Shares accesibles sin autenticación.  
- Enumeración de usuarios → facilita fuerza bruta.  
- Permisos de escritura → posibilidad de subir shells o malware.  
- Uso de SMBv1 → vulnerabilidades graves (EternalBlue, etc).  
