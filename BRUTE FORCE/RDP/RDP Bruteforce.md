# Hydra

`hydra -L /usr/share/metasploit-framework/data/worldlists/common_users.txt -P /usr/share/metasploit-framework/data/worldlists/common_passwords.txt rdp://<IP> -s 3389` -t 2

Params:
-l : En minúscula el nombre de usuario en concreto
-L: En mayúscula la lista de usuarios
-p: contraseña
-P: lista de contraseñas
-s: puerto dónde corre el servicio en este caso RDP
-t: Número de tasks en paralelo. Default: 16. Reducir para evitar hacer un DOS al servicio. 

## Conexión al servicio RDP

#### Por xfreerdp:
`xfreerdp /u:administrator /p:password /v:<ip>:<port>ç
El puerto por defecto es 3389 pero se puede especificar en formato socket.
