# 📡 FTP y TFTP

## 🔹 FTP (File Transfer Protocol)
- Uno de los protocolos más antiguos de Internet. Funciona en la **capa de aplicación** del stack TCP/IP (igual que HTTP, POP).  
- Utiliza **dos canales**:
  - **Control (TCP/21)**: comandos y respuestas.  
  - **Datos (TCP/20)**: transmisión de archivos.  

✅ Soporta **reanudación** si la conexión se corta.  

### 🔀 Modos de conexión
- **Activo**: cliente abre conexión al puerto 21 → el servidor responde usando un puerto alto en el cliente. Problema: bloqueado por firewalls.  
- **Pasivo**: el servidor abre un puerto y el cliente inicia la conexión → más compatible con firewalls.  

### 🔑 Autenticación
- Normalmente requiere **usuario/contraseña**.  
- Puede habilitarse **login anónimo** (`anonymous`), con permisos limitados.  
- **Inseguro**: el tráfico va en **texto plano** y puede ser esnifado.  

---

## ⚙️ Servidores FTP – vsFTPd (Linux)
Archivo de configuración: `/etc/vsftpd.conf`  

Parámetros básicos:
- `anonymous_enable=NO` → deshabilita login anónimo.  
- `local_enable=YES` → permite login de usuarios locales.  
- `xferlog_enable=YES` → habilita logging.  
- `/etc/ftpusers` → lista de usuarios bloqueados.  

### 🚩 Configuraciones peligrosas
- `anonymous_enable=YES` → login anónimo habilitado.  
- `anon_upload_enable=YES` → permite subir archivos.  
- `anon_mkdir_write_enable=YES` → permite crear directorios.  
- `write_enable=YES` → habilita comandos de escritura (STOR, DELE, MKD, etc).  

Con estas opciones un atacante puede:
- Descargar información sensible.  
- Subir archivos maliciosos y conseguir **RCE** si el FTP está vinculado a un webserver.  

---

## 🔎 Enumeración y Footprinting

### 📂 Cliente FTP estándar
- Listar directorios:  
  ```bash
  ftp> ls
  ftp> ls -R      # listado recursivo
  ftp> status     # muestra configuración de la sesión
  ftp> debug      # activa salida detallada
  ftp> trace      # traza los comandos enviados/recibidos
  ftp> ls -la      # Muestra directorios y archivos ocultos
  ```
- Descargar archivos:  
  ```bash
  ftp> get file.txt
  ```
- Subir archivos:  
  ```bash
  ftp> put test.txt
  ```
- Descargar todo el contenido:  
  ```bash
  wget -m --no-passive ftp://anonymous:anonymous@IP
  ```

---

### 🛠️ Escaneo con Nmap
- Actualizar scripts NSE:
  ```bash
  sudo nmap --script-updatedb
  ```
- Escaneo básico de FTP:
  ```bash
  sudo nmap -sV -p21 -sC -A IP
  ```
- Scripts útiles:
  - `ftp-anon` → detecta login anónimo.  
  - `ftp-syst` → información del servidor con STAT.  
  - `ftp-brute` → ataque de fuerza bruta.  
  - `ftp-vsftpd-backdoor`, `ftp-proftpd-backdoor` → buscan exploits conocidos.  

- Ver comandos enviados con traza:
  ```bash
  sudo nmap -sV -p21 -sC -A IP --script-trace
  ```

---

### 🔐 Enumeración con OpenSSL
Si el servidor soporta **TLS/SSL**:
```bash
openssl s_client -connect IP:21 -starttls ftp
```
Permite ver:
- Certificado SSL (CN, OU, email).  
- Hostname de la máquina.  
- Posibles localizaciones geográficas.  

Esto puede dar pistas de **nombres internos de dominio** o **emails de admins**.  

---

### 🖥️ Interacción manual con Netcat / Telnet
- **Netcat**:
  ```bash
  nc -nv IP 21
  ```
  Luego podemos enviar comandos FTP manualmente:  
  ```
  USER anonymous
  PASS anonymous
  LIST
  ```
- **Telnet**:
  ```bash
  telnet IP 21
  ```
  Igual que con `nc`, permite interactuar con comandos básicos.  

Esto es útil para:
- Ver el **banner** del servicio.  
- Probar credenciales simples.  
- Detectar respuestas personalizadas del servidor.  

---

## 🔹 TFTP (Trivial FTP)
- Variante más simple, **basada en UDP** (no TCP).  
- **No tiene autenticación** ni listados de directorios.  
- Solo puede acceder a archivos con permisos globales de lectura/escritura.  
- Uso típico: **entornos locales seguros** (ej. arranque PXE).  

### 📌 Comandos básicos
```bash
connect <host> [puerto]   # conectar al servidor
get file.txt              # descargar
put file.txt              # subir
status                    # ver configuración
verbose                   # salida detallada
quit                      # salir
```

⚠️ Por su falta de seguridad, **no debería exponerse a Internet**.  
