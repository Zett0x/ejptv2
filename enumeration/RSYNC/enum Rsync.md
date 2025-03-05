
Listar recursos:
`rsync --list-only rsync://192.228.190.3/`

![[Pasted image 20250305150945.png]]

Acceder a un recurso:
`rsync -av rsync://192.228.190.3/backupwscohen`
![[Pasted image 20250305151010.png]]

Descargarlo en el directorio actual:
`rsync -av rsync://192.228.190.3/backupwscohen .`
``