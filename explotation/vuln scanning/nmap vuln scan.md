Una vez sabemos el servicio y versión que corre en un target, podemos usar nmap scripts para analizar si es vulnerable a x exploit.

Primero debemos saber la ubicación por defecto de los scripts de nmap:

`ls -la /usr/share/nmap/scripts`

Si por ejemplo ya sabemos que el target está corriendo un servidor http, podemos filtrar los scripts:
`ls -la /usr/share/nmap/scripts | grep http`

Si sospechamos que el servidor, al estar corriendo un apache y scripts cgi, queremos comprobar si es vulnerable a shellshock vulnerability:
![[Pasted image 20250213123058.png]]

`nmap -p80 --script=http-shellshock --script-args "http-shellshock.uri=/gettime.cgi" <ip>`