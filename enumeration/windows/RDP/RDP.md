# RDP (Remote Desktop Protocol)

- Protocolo gráfico de Microsoft para controlar remotamente un sistema Windows.  
- Usa normalmente el **puerto TCP/3389** (también puede usar UDP/3389).  
- Cifra la comunicación con TLS/SSL desde Windows Vista, pero muchos sistemas aceptan aún cifrado débil o certificados **self-signed**.  
- Servicio instalado por defecto en servidores Windows.  
- Normalmente requiere NLA (Network Level Authentication).

## Footprinting
```bash
nmap -sV -sC -p3389 --script rdp* 10.129.201.248
```

## Herramientas
- **rdp-sec-check.pl**  
- **xfreerdp / rdesktop / Remmina**

## Ejemplo de conexión
```bash
xfreerdp /u:usuario /p:"contraseña" /v:10.129.201.248
```
