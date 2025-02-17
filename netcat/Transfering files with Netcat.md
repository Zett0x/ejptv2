## Tranfering files with netcat

Necesitamos poner en escucha netcat en el sistema donde queramos RECIBIR el fichero, y conectarnos desde el sistema que queramos ENVIAR el fichero.

#### Server ( receiving file):
`nc -nvlp 3333 > test.txt` 
Eso significa ponte en escucha por el puerto tcp 3333, no hagas resolucion dns y lo que recibas lo metes en un fichero test.txt

#### Client (sending file):
echo "hola"> test.txt
`nc -nv <ip> 3333 < test.txt`

Este proceso se puede usar con binarios también, no sólamente con ficheros de texto.