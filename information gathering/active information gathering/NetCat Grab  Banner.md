A veces con nmap y el parámetro -sV, no nos da la versión exacta del servicio que está corriendo en la máquina víctima. Entonces siempre podemos usar netcat para obtener el banner.

## Command
`nc <ip> <port>`

![[Pasted image 20250213120430.png]]

Esta información nos servirá para buscar vulnerabilidades de ese servicio ![[Pasted image 20250213120534.png]]

También podemos usar el cliente del servicio, en este caso ssh, o en un servidor ftp el cliente ftp o en smb el cliente smbclient para intentar conectarnos y que nos muestre el banner a ver si da algún tipo de info.