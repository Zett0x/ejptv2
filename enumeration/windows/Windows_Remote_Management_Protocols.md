# Windows Remote Management Protocols

Los servidores Windows pueden ser administrados local o remotamente.  
A partir de Windows Server 2016 la **gestión remota está habilitada por defecto**.  
Los principales protocolos para administración remota son:

- **RDP (Remote Desktop Protocol)**  
- **WinRM (Windows Remote Management)**  
- **WMI (Windows Management Instrumentation)**  

---

## 🔹 RDP (Remote Desktop Protocol)
- Protocolo gráfico de Microsoft para controlar remotamente un sistema Windows.  
- Usa normalmente el **puerto TCP/3389** (también puede usar UDP/3389).  
- Cifra la comunicación con TLS/SSL desde Windows Vista, pero muchos sistemas aceptan aún cifrado débil o certificados **self-signed** (vulnerables a MITM).  
- Servicio instalado por defecto en servidores Windows.  
- Requiere que el firewall/NAT permitan el acceso y, normalmente, NLA (**Network Level Authentication**) activado.

### Footprinting
```bash
nmap -sV -sC -p3389 --script rdp* 10.129.201.248
```

Ejemplo de info obtenida:
- Target_Name, NetBIOS, DNS, versión del sistema.
- Si NLA está habilitado.
- Cifrado soportado.

### Herramientas
- **rdp-sec-check.pl** → script de Cisco para verificar niveles de cifrado RDP.  
- **xfreerdp / rdesktop / Remmina** → clientes para conectarse desde Linux.

### Comando ejemplo
```bash
xfreerdp /u:usuario /p:"contraseña" /v:10.129.201.248
```

---

## 🔹 WinRM (Windows Remote Management)
- Basado en **SOAP**, permite ejecutar comandos de forma remota (CLI).  
- Usa los puertos **5985 (HTTP)** y **5986 (HTTPS)**.  
- **Habilitado por defecto desde Windows Server 2012**, en clientes debe configurarse manualmente.  
- Permite ejecución de comandos remotos mediante **WinRS** o **PowerShell Remoting**.  
- Requiere firewall abierto y configuración adecuada.

### Footprinting
```bash
nmap -sV -sC -p5985,5986 10.129.201.248
```

### Herramientas
- **PowerShell** (`Test-WsMan`).  
- **Evil-WinRM** → herramienta de pentesting.

### Comando ejemplo
```bash
evil-winrm -i 10.129.201.248 -u usuario -p contraseña
```

---

## 🔹 WMI (Windows Management Instrumentation)
- Implementación de Microsoft del estándar **WBEM/CIM**.  
- Permite acceso de lectura y escritura a casi toda la configuración de un Windows.  
- Usado para administración remota de PCs y servidores.  
- Incluye repositorios y múltiples componentes.  
- Normalmente se accede vía **PowerShell, VBScript o WMIC**.  

### Footprinting
- Inicia en **TCP/135**, luego cambia a un puerto dinámico.  
- Se puede abusar con herramientas como **wmiexec.py** (Impacket).

### Comando ejemplo
```bash
wmiexec.py usuario:contraseña@10.129.201.248 "hostname"
```

---

## ✅ Resumen comparativo

| Protocolo | Puerto(s) | Uso principal | Herramienta ataque |
|-----------|-----------|---------------|--------------------|
| **RDP**  | TCP/3389, UDP/3389 | Acceso remoto por GUI | `xfreerdp`, `rdp-sec-check` |
| **WinRM** | TCP/5985, 5986 | Ejecución de comandos remotos | `evil-winrm` |
| **WMI**  | TCP/135 → dinámico | Administración total | `wmiexec.py` |
