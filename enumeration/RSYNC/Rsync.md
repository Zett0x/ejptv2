# Rsync

Herramienta para **sincronización y copia de archivos** de manera rápida y eficiente, tanto localmente como entre hosts remotos.

- **Puerto**: TCP/873  
- Herramienta para copias y sincronización de ficheros.  
- Puede funcionar sin autenticación.  
- Usa algoritmo de **delta-transfer** (solo transmite cambios).  
- Puede integrarse con **SSH** para transferencia segura.  

## Riesgos
- Shares accesibles sin credenciales.  
- Fugas de ficheros sensibles (`.ssh/`, backups).  
- Reutilización de credenciales.  

## Comandos útiles
```bash
# Escaneo del servicio
nmap -sV -p 873 <ip>

# Conexión manual con Netcat
nc -nv <ip> 873
#list

# Enumerar un share
rsync -av --list-only rsync://<ip>/<share>

# Descargar todo el contenido del share
rsync -av rsync://<ip>/<share> .

# Usar Rsync a través de SSH
rsync -av -e ssh usuario@<ip>:/ruta /localdir
```
