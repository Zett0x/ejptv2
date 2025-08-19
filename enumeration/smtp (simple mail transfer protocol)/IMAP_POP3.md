# 📬 IMAP / POP3 – Resumen

Los protocolos **IMAP (Internet Message Access Protocol)** y **POP3 (Post Office Protocol v3)** permiten a los clientes acceder a correos almacenados en un servidor de correo.  

- **IMAP (143 / 993 SSL):**  
  - Permite gestionar los correos directamente en el servidor.  
  - Soporta carpetas y subcarpetas.  
  - Varios clientes pueden estar sincronizados al mismo tiempo.  
  - Los correos permanecen en el servidor hasta ser eliminados.  
  - Ideal para múltiples dispositivos (móvil, PC, webmail).  

- **POP3 (110 / 995 SSL):**  
  - Mucho más simple.  
  - Solo permite listar, descargar y borrar correos.  
  - Por defecto descarga los mensajes y los elimina del servidor (aunque algunos clientes permiten dejar copia).  
  - Más usado en sistemas antiguos o configuraciones ligeras.  

---

## 📌 Puertos
- **IMAP:** 143 (texto plano), 993 (TLS/SSL).  
- **POP3:** 110 (texto plano), 995 (TLS/SSL).  

⚠️ Ambos transmiten **credenciales y correos en texto plano** si no se usa SSL/TLS.  

---

## 📌 Comandos principales
### IMAP
- `LOGIN usuario pass` → autenticación.  
- `LIST "" *` → listar todas las carpetas.  
- `CREATE` / `DELETE` / `RENAME` → administrar buzones.  
- `SELECT INBOX` → abrir bandeja.  
- `FETCH <ID> all` → obtener un correo.  
- `CLOSE` → marcar como eliminados los mensajes con flag `Deleted`.  
- `LOGOUT` → salir.  

### POP3
- `USER usuario` → identifica usuario.  
- `PASS clave` → autenticación.  
- `STAT` → número de correos.  
- `LIST` → listado de correos con tamaño.  
- `RETR <id>` → recuperar correo.  
- `DELE <id>` → borrar correo.  
- `CAPA` → capacidades del servidor.  
- `QUIT` → salir.  

---

## 📌 Configuraciones peligrosas
- `auth_debug`, `auth_verbose` → registro excesivo de credenciales.  
- `auth_debug_passwords`, `auth_verbose_passwords` → pueden registrar contraseñas en texto claro.  
- `auth_anonymous_username` → permite acceso anónimo.  

❌ Una mala configuración puede dejar buzones accesibles sin autenticación o exponer contraseñas en logs.  

---

## 📌 Footprinting y enumeración
- **Nmap** detecta versiones y capacidades:  
  ```bash
  nmap -sV -p110,143,993,995 -sC <IP>
  ```
  Ejemplo de salida:  
  ```
  110/tcp open  pop3     Dovecot pop3d
  143/tcp open  imap     Dovecot imapd
  993/tcp open  ssl/imap Dovecot imapd
  995/tcp open  ssl/pop3 Dovecot pop3d
  ```

- Con certificados SSL, se pueden obtener datos como:  
  - **CN (FQDN):** mail1.inlanefreight.htb  
  - **Organization:** Inlanefreight  
  - **Email del admin:** cry0l1t3@inlanefreight.htb  

---

## 📌 Herramientas para interactuar
- **cURL:**  
  ```bash
  curl -k 'imaps://10.129.14.128' --user user:pass
  ```
- **openssl s_client:**  
  ```bash
  openssl s_client -connect 10.129.14.128:993
  openssl s_client -connect 10.129.14.128:995
  ```
- **ncat** → también permite conexión manual con TLS.  

---

## 📌 Riesgos
- Con credenciales válidas, un atacante puede **leer y enviar correos**.  
- Correos corporativos contienen información sensible, contraseñas, contratos, etc.  
- Si se reutilizan credenciales débiles (ej: `robin:robin`), el acceso es trivial.  

---

## ✅ Resumen para pentester
1. Escanear puertos 110/995 y 143/993.  
2. Revisar certificados → organización, CN, emails.  
3. Intentar autenticación con credenciales descubiertas.  
4. Usar `cURL`, `openssl` o clientes IMAP/POP3 para navegar el buzón.  
5. Buscar flags, contraseñas o información sensible.  
