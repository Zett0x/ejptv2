Si tenemos hashes NTLM de cualquier usuario, podemos utilizarlos para logearnos vía SMB. Para ello podemos utilizar dos herramientas:
- MetaSploit Psexec module: `exploit/windows/smb/psexec`
- crackmapexec

Para realizar este ataque, necesitamos tanto el hash NTLM que obtuvimos mediante mimikatz o el módulo kiwi cómo el hash LM, que se puede obtener usando el comando `hashdump` si estamos en una consola de meterpreter. Podemos comprobar que el hash LM es común a todos los usuarios:
![[Pasted image 20250210171219.png]]


## MetaSploit module Psexec

En el parámetro SMBPass podemos poner una contraseña en texto plano o, en este caso, el hash LM + NTLM. En formato LM:NTLM

Si sale el siguiente log, cambiar el target a Command:
![[Pasted image 20250210171939.png]]

Si sigue sin funcionar poner set target Native\ upload y ver si funciona. En el caso del vídeo ya funciona con eso.

NOTA: Al usar el módulo de metasploit psexec, asegurarse de no poner como LPORT en las opciones del payload, el mismo puerto que otra sesión activa ya que daría error al estar en uso ese puerto ya.

## Crackmapexec

Con esta herramienta, con el parámetro -H podemos especificarle el uso del hash NTLM. En este caso no es necesario el hash LM
![[Pasted image 20250210172608.png]]

`crackmapexec smb <ip> -u Administrator -H <hash NTLM>`

NOTA: añadiendo el parámetro -x "command" podemos ejecutar un comando:
`crackmapexec smb <ip> -u Administrator -H <hash NTLM> -x "whoami"`



NOTA: Podemos probar hashes NTLM usando el módulo` auxiliary/scanner/smb/smb_login`