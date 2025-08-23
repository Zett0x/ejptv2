# R-Services

Son un conjunto de servicios Unix antiguos para acceso remoto y ejecución de comandos sobre TCP/IP.

- **Puertos**:
  - 512/tcp → rexec  
  - 513/tcp → rlogin, rwho  
  - 514/tcp → rsh  

## Problemas
- Transmisión en **texto claro** (incluyendo contraseñas).  
- Basados en confianza → inseguros.  
- Reemplazados por SSH, pero aún presentes en entornos legacy.  

## Archivos de confianza
- `/etc/hosts.equiv` → configuración global.  
- `~/.rhosts` → configuración por usuario.  

### Sintaxis
```
<usuario> <hostname/ip>
```

### Ejemplos
```bash
# Usuario htb-student puede acceder desde 10.0.17.5
htb-student     10.0.17.5

# Cualquier usuario puede acceder desde 10.0.17.10
+               10.0.17.10

# PELIGRO: cualquier usuario desde cualquier host
+               +
```

## Comandos útiles
```bash
# Escaneo de R-services
nmap -sV -p 512,513,514 <ip>

# Conectar con rlogin
rlogin <ip> -l <usuario>

# Conectar con rsh y ejecutar comando
rsh <ip> -l <usuario> "id"

# Conectar con rexec (pedirá password)
rexec <ip> -l <usuario> "id"

# Listar usuarios conectados en la red
rwho
rusers -al <ip>
```
