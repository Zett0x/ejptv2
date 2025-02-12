Este servicio se creó para facilitar la administración de sistemas windows remotamente. Puede ser en la misma red local o en red pública, es decir, de algún servidor de internet que lo tenga habilitado.

default port: 5985

## nmap
	nmap -sVC -p5985 -T4 -Pn <ip>

El servicio reportado por nmap será http y la versión no saldrá WinRM o algo así sino:
![[Pasted image 20250205174153.png]]



