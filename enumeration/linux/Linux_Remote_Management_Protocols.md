# Linux Remote Management Protocols

En Linux existen múltiples protocolos y servicios para la **administración remota** de sistemas.  
Aunque facilitan la gestión, muchas veces están mal configurados y se convierten en un **objetivo muy atractivo en pentesting**.  

---

## 1. SSH (Secure Shell)

- **Puerto**: TCP/22  
- **Versiones**:
  - **SSH-1**: inseguro, vulnerable a MITM, sin forward secrecy.  
  - **SSH-2**: versión actual, cifrado fuerte, segura y estable.  
- **Usos**: administración remota, envío de comandos, copias de ficheros (scp, sftp), port forwarding, túneles.  

### Métodos de autenticación
- Password  
- Public Key (clave pública/privada)  
- Host-based  
- Keyboard-interactive  
- Challenge-response  
- GSSAPI  

### Vulnerabilidades históricas de OpenSSH
- **CVE-2016-3115** → X11Forwarding en OpenSSH 7.2p1 → command injection (**RCE**).  
- **CVE-2020-14145** → debilidad en validación de integridad en primera conexión (**MITM posible**).  
- **CVE-2018-15473** → user enumeration (podías diferenciar usuarios válidos e inválidos).  
- **Protocol 1** → obsoleto e inseguro.  

### Configuración peligrosa (`/etc/ssh/sshd_config`)
- `PasswordAuthentication yes` → brute force.  
- `PermitEmptyPasswords yes` → contraseñas vacías.  
- `PermitRootLogin yes` → login directo como root.  
- `Protocol 1` → inseguro.  
- `X11Forwarding yes` → potencial RCE.  
- `AllowTcpForwarding yes` / `PermitTunnel yes` → pivoting.  
- `DebianBanner yes` → fuga de información.  

### Comandos útiles
```bash
# Ver configuración del servidor
cat /etc/ssh/sshd_config | grep -v "#" | sed -r '/^\s*$/d'

# Conexión normal
ssh usuario@ip

# Forzar método de autenticación
ssh -o PreferredAuthentications=password usuario@ip

# Enumerar algoritmos y banner (verbose)
ssh -v usuario@ip

# Fingerprinting de versión y algoritmos con ssh-audit
git clone https://github.com/jtesta/ssh-audit.git
./ssh-audit.py <ip>
```

---

## 2. Rsync

- **Puerto**: TCP/873  
- Herramienta para copias y sincronización de ficheros.  
- Puede funcionar sin autenticación (según configuración).  
- Algoritmo de **delta-transfer** → solo transmite cambios.  
- Puede usarse con **SSH** para transferencia segura.  

### Riesgos
- Shares accesibles sin credenciales.  
- Exposición de directorios sensibles (`.ssh/`, backups, config files).  
- Reutilización de credenciales entre Rsync y otros servicios.  

### Comandos útiles
```bash
# Escaneo de servicio
nmap -sV -p 873 <ip>

# Conexión manual con Netcat
nc -nv <ip> 873
#list

# Enumerar un share
rsync -av --list-only rsync://<ip>/<share>

# Descargar todos los ficheros del share
rsync -av rsync://<ip>/<share> .

# Usar Rsync a través de SSH
rsync -av -e ssh usuario@<ip>:/ruta /localdir
```

---

## 3. R-Services

- **Puertos**:
  - 512/tcp → rexec  
  - 513/tcp → rlogin, rwho  
  - 514/tcp → rsh  
- **Problema**: comunicación en **texto claro** (usuarios y contraseñas viajan sin cifrar).  
- Usados en sistemas Unix antiguos (Solaris, HP-UX, AIX).  
- Reemplazados por SSH, pero aún aparecen en entornos legacy.  

### Archivos de confianza
- `/etc/hosts.equiv` → configuración global (todos los usuarios).  
- `~/.rhosts` → configuración por usuario.  

**Sintaxis**:
```
<usuario> <hostname/ip>
```

Ejemplos:
```bash
# Usuario htb-student puede acceder desde 10.0.17.5
htb-student     10.0.17.5

# Cualquier usuario puede acceder desde 10.0.17.10
+               10.0.17.10

# PELIGRO: cualquier usuario desde cualquier host (sin password)
+               +
```

⚠️ Con `+ +` se permite login sin contraseña **a cualquier usuario válido en el sistema** desde cualquier máquina.  

### Comandos útiles
```bash
# Escaneo de R-services
nmap -sV -p 512,513,514 <ip>

# Conectar con rlogin
rlogin <ip> -l <usuario>

# Conectar con rsh y ejecutar comando
rsh <ip> -l <usuario> "id"

# Conectar con rexec (pedirá usuario/contraseña)
rexec <ip> -l <usuario> "id"

# Listar usuarios conectados en la red
rwho
rusers -al <ip>
```

---

## Puntos Clave

- **SSH**: usar solo SSH-2, claves públicas, deshabilitar root y passwords. Revisar versiones vulnerables.  
- **Rsync**: revisar si hay shares abiertos, posibles credenciales reutilizadas o directorios sensibles.  
- **R-Services**: obsoletos e inseguros; si se encuentran, casi siempre implican login sin password por `.rhosts` o `hosts.equiv`.  
- En pentesting:  
  - Enumerar versiones (banners, `ssh-audit`).  
  - Buscar configuraciones débiles (`PermitRootLogin`, `+ +`).  
  - Intentar login con usuarios válidos del sistema.  
  - Descargar ficheros sensibles (rsync, rcp).  
