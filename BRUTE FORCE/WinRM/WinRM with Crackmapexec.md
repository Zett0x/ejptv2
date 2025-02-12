Es importante poner de usuario, en un principio, al usuario administrator porque es un usuario presente en todos los sistemas windows además de que si conseguimos descubrir la contraseña, tendremos acceso con privilegios elevados.

Command:
	crackmapexec winrm -u administrator -p <password/pass_lists>


### Ejecutar comandos con crackmapexec
Requiere de credenciales válidas:
	crackmapexec winrm -u administrator -p thispasswordiscool -x <command>