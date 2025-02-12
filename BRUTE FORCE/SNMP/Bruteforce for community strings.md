
El puerto utilizado por el servicio es mediante el protocolo de transporte UDP en el puerto 161.
En la version 1 y 2 usan lo que se conoce como community strings, y se transportan sin cifrar.
# with Nmap

`nmap -sU -sV -p161 --script=snmp-brute <ip>`
![[Pasted image 20250212154234.png]]

Usar el parámetro -sV para ver la versión de SNMP