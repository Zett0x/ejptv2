# WMI (Windows Management Instrumentation)

- Implementación de Microsoft del estándar **WBEM/CIM**.  
- Permite acceso de lectura y escritura a la configuración de un sistema Windows.  
- Usado para administración remota de PCs y servidores.  
- Se accede principalmente con **PowerShell, VBScript o WMIC**.

## Footprinting
- Inicia en **TCP/135**, después se mueve a un puerto dinámico.  
- Puede abusarse con herramientas como **wmiexec.py** de Impacket.

## Ejemplo de conexión
```bash
wmiexec.py usuario:contraseña@10.129.201.248 "hostname"
```
