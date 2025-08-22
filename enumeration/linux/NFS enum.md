NFS es un servicio compatible con dispositivos Linux para compartir archivos y directorios.

Puerto por defecto: 2049
## Mostrar recursos compartidos

`showmount -e <ipservidor nfs`>
![[Pasted image 20250314112221.png]]

## Mostrar fichero donde se listan los directorios compartidos

`cat /etc/exports`
![[Pasted image 20250314113858.png]]

Si las opciones no_root_squash están seteadas, quiere decir que cuando accedemos a los directorios compartidos como root, y creamos por ejemplo una shell.c con SUID activado, desde un usuario sin privilegios en la máquina dónde está corriendo el servidor NFS y se está compartiendo el recurso, ejecutamos el binario ./shell con el setuid activado y cuyos propietarios sean ROOT, y escalaremos privilegios.

**Ver en post-exploitation/privilege escalation/linux el NFS example.**
## Montar los recursos compartidos

````shell-session

sudo mount -t nfs <ip_servidor_nfs>:/<nombre recurso> /<ruta local donde montar el recurso>
````
![[Pasted image 20250314112303.png]]
