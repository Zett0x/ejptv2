# WinRM (Windows Remote Management)

- Protocolo de gestión remota basado en **SOAP**.  
- Usa **TCP 5985 (HTTP)** y **5986 (HTTPS)**.  
- Habilitado por defecto en Windows Server 2012 y superior.  
- Permite ejecución de comandos remotos mediante **WinRS** y **PowerShell Remoting**.

## Footprinting
```bash
nmap -sV -sC -p5985,5986 10.129.201.248
```

## Herramientas
- **PowerShell** (`Test-WsMan`).  
- **Evil-WinRM**

## Ejemplo de conexión
```bash
evil-winrm -i 10.129.201.248 -u usuario -p contraseña
```
