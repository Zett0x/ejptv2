
# SSH (Secure Shell)

Servicio para administración remota en servidores Linux de forma nativa, aunque también se puede instalar en sistemas windows.

- **Puerto**: TCP/22  
- **Versiones**:
  - **SSH-1**: inseguro, vulnerable a MITM, sin forward secrecy.  
  - **SSH-2**: versión actual, cifrado fuerte, segura y estable.  

## Usos
- Administración remota (CLI o GUI).  
- Transferencia de archivos (scp, sftp).  
- Port forwarding y túneles.  

## Métodos de autenticación
- Password  
- Public Key  
- Host-based  
- Keyboard-interactive  
- Challenge-response  
- GSSAPI  

## Vulnerabilidades históricas
- **CVE-2016-3115** → X11Forwarding en OpenSSH 7.2p1 → command injection (RCE).  
- **CVE-2020-14145** → falla en verificación de integridad inicial → posible MITM.  
- **CVE-2018-15473** → user enumeration.  
- **Protocol 1** → inseguro y obsoleto.  

## Configuración peligrosa (`/etc/ssh/sshd_config`)
- `PasswordAuthentication yes` → brute force.  
- `PermitEmptyPasswords yes` → contraseñas vacías.  
- `PermitRootLogin yes` → login directo como root.  
- `Protocol 1` → inseguro.  
- `X11Forwarding yes` → potencial RCE.  
- `AllowTcpForwarding yes` / `PermitTunnel yes` → pivoting.  
- `DebianBanner yes` → fuga de versión.  

## Comandos útiles
```bash
# Ver configuración del servidor
cat /etc/ssh/sshd_config | grep -v "#" | sed -r '/^\s*$/d'

# Conexión normal
ssh usuario@ip

# Forzar método de autenticación
ssh -o PreferredAuthentications=password usuario@ip

# Enumerar algoritmos y banner (verbose)
ssh -v usuario@ip

# Fingerprinting con ssh-audit
git clone https://github.com/jtesta/ssh-audit.git
./ssh-audit.py <ip>
```
