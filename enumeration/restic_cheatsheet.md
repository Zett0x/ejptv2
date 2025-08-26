# 📌 Restic Commands Cheat Sheet

## 🔹 Inicializar repositorio
```bash
restic -r /ruta/al/repositorio init
```

## 🔹 Crear backup
```bash
restic -r /ruta/al/repositorio backup /ruta/a/respaldar
```

Con etiqueta:
```bash
restic -r /ruta/al/repositorio backup /etc --tag config
```

## 🔹 Listar snapshots
```bash
restic -r /ruta/al/repositorio snapshots
restic -r /ruta/al/repositorio snapshots --json   # en JSON
```

## 🔹 Listar archivos de un snapshot
```bash
restic -r /ruta/al/repositorio ls latest
restic -r /ruta/al/repositorio ls <snapshot_id>
```

## 🔹 Dump (ver contenido de un archivo)
```bash
restic -r /ruta/al/repositorio dump latest ruta/del/archivo
restic -r /ruta/al/repositorio dump latest etc/passwd > passwd.txt
```

## 🔹 Restaurar backups
```bash
restic -r /ruta/al/repositorio restore latest --target /tmp/restore
restic -r /ruta/al/repositorio restore <snapshot_id> --target /tmp/restore --include /etc/hosts
```

## 🔹 Comparar snapshots
```bash
restic -r /ruta/al/repositorio diff <snapshot_id1> <snapshot_id2>
```

## 🔹 Información y estadísticas
```bash
restic -r /ruta/al/repositorio stats
restic -r /ruta/al/repositorio stats --mode raw-data
```

## 🔹 Limpiar repo
```bash
restic -r /ruta/al/repositorio forget --keep-last 5 --prune
```

## 🔹 Explorar bajo nivel
```bash
restic -r /ruta/al/repositorio cat snapshot <snapshot_id>
restic -r /ruta/al/repositorio cat blobs
```

---

⚡ **Tips**:  
- Usa `latest` en lugar de ID para el último snapshot.  
- Puedes exportar contraseña:  
  ```bash
  export RESTIC_PASSWORD=tu_password
  ```  
- Para repos remotos:  
  ```bash
  restic -r sftp:user@host:/path snapshots
  restic -r b2:bucketname:/path backup ~/data
  ```
