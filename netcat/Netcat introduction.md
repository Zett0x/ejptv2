Se puede usar tanto como cliente como servidor para establecer/recibir conexiones TCP o UDP.
Nota: Windows no viene con netcat por defecto instalado por lo que podemos transferirselo. En kali tenemos windows pre compiled binaries en:
`/usr/share/windows-binaries/nc.exe`

## Conectarnos a un puerto TCP

`nc -nv 10.10.10.1 80`
params:
- -n: Que no aplique resolución dns
- -v: Verbose, que muestre resultado.

### Conexión TCP exitosa, puerto abierto:
![[Pasted image 20250217152440.png]]

### Conexión TCP no exitosa, puerto cerrado o filtrado por firewall:
![[Pasted image 20250217152518.png]]

## Conexión puerto UDP

`nc -nvu 10.10.10.1 139`

## Creando un Listener
`nc -nvlp`443
params: 
- -n: No resolucion dns
- -v: Verbose
- -l: Listener, estamos escuchando peticiones
- -p: El puerto en el que escuchamos
