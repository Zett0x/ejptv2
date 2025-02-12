# **📂 Enumeración SMB**

SMB permite compartir archivos en red. La enumeración SMB puede revelar usuarios, recursos compartidos y vulnerabilidades.

### 1️⃣ Metasploit

🔍 Identificar versión de SMB
`use auxiliary/scanner/smb/smb_version`
`set RHOSTS <IP>`
`run`

📁 Listar recursos compartidos

`use auxiliary/scanner/smb/smb_enumshares`
`set RHOSTS <IP>`
`run`

👤 Enumerar usuarios

`use auxiliary/scanner/smb/smb_enumusers`
`set RHOSTS <IP>`
`run`

**Enumerar usuarios con nmap**
`nmap -p 445 --script smb-enum-users.nse <ip>`

🔑 Probar credenciales
`use auxiliary/scanner/smb/smb_login 
`set RHOSTS <IP>` 
`set USER_FILE users.txt` 
`set PASS_FILE passwords.txt run`

## 2️⃣smb client

📂 Listar recursos compartidos
`smbclient -L //<IP> -N

📁 Acceder a un recurso
`smbclient //<IP>/sharename -U usuario

**📜 Descargar/Subir archivos**
`get <archivo>  # Descargar
`put <archivo>  # Subir`

## 3 smbmap
Listar recursos y permisos sobre esos recursos de un usuario:
`smbmap -u username -p password -H <ip>`

## 3️⃣ CrackMapExec

🕵️‍♂️ **Escaneo SMB**
`crackmapexec smb <IP>

📁 Listar recursos compartidos
`crackmapexec smb <IP> --shares
`
🔑 Probar credenciales
`crackmapexec smb <IP> -u usuario -p contraseña`

💀 Dump de hashes
`crackmapexec smb <IP> -u usuario -p contraseña --sam

### **✅ Probar credenciales SMB con Hydra**

`hydra -L users.txt -P passwords.txt <ip> smb
`



`



`
`

`
