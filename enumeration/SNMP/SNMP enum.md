
Si al escanear un host con Nmap vemos **el puerto 161/UDP abierto**, significa que:

- El host **tiene un agente SNMP activo** y responde a consultas.
- Es posible que el dispositivo esté **exponiendo información sensible**, dependiendo de su configuración SNMP.

# with nmap

Podemos ejecutar todos los scripts de nmap para enumerar el servicio:
`nmap -sU -p 161 script=snmp-* demo.ine.local -oN snmp_info`

Nos mostrará mucha información, entre ella usuarios:
![[Pasted image 20250212155044.png]]

Podemos ejecutar ahora fuerza bruta con esos usuarios
