**PsExec** es el **reemplazo de telnet** por microsoft para ejecutar comandos de forma remota o ganar una shell en un sistema windows. No podemos usar esta herramienta en **linux**, pero hay un script en python para esto: **psexec.py**
Es necesario usar un usuario administrador para poder utilizar este módulo.

Utiliza el servicio **smb (445)** para la autenticación por lo que necesitaremos credenciales válidas.

`psexec.py Administrator@1.1.1.1 cmd.exe`

Si quieres meterpreter session, mejor usar un modulo de metasploit:
**exploit/windows/smb/psexec**
Este módulo sube un malware al target que puede ser detectado por el antivirus. Para no arriesgarse, usar psexec.py es suficiente
## Obtención de credenciales

### Fuerza bruta
Podemos usar el módulo de metasploit **auxiliary(scanner/smb/smb_login)** especificamos un usuario o lista de usuarios y contraseña o lista de contraseñas.

