# 📧 SMTP – Footprint

El **Simple Mail Transfer Protocol (SMTP)** es el protocolo encargado de **enviar correos electrónicos** a través de redes IP.  

Puede funcionar:  
- **Cliente ↔ Servidor** (cuando un usuario envía correo a su servidor).  
- **Servidor ↔ Servidor** (cuando un servidor entrega correo al servidor destino).  

Normalmente se combina con **IMAP o POP3** para la **recepción de correos**.  

---

## 📌 Puertos
- **25/TCP** → envío entre servidores (por defecto).  
- **587/TCP** → envío autenticado (con `STARTTLS`).  
- **465/TCP** → envío con SSL/TLS (obsoleto, pero aún usado).  

⚠️ Sin cifrado, SMTP transmite todo en **texto plano**, incluyendo **credenciales**.  

---

## 📌 Componentes y Flujo
1. **MUA (Mail User Agent)** – Cliente de correo (ej: Thunderbird, Outlook).  
2. **MSA (Mail Submission Agent)** – Valida origen, puede actuar como “relay” interno.  
3. **MTA (Mail Transfer Agent)** – Encargado de enviar/reenviar correos (ej: Postfix, Sendmail).  
4. **MDA (Mail Delivery Agent)** – Entrega en el buzón del usuario (ej: Dovecot).  
5. **POP3/IMAP** – Permiten que el usuario reciba sus correos.  

> 🔑 El problema surge cuando un **MTA** mal configurado actúa como **Open Relay** y permite que cualquiera envíe correos falsos.  

---

## 📌 Comandos SMTP importantes
- `HELO <host>` → inicia sesión con el nombre de host (no un correo).  
- `EHLO <host>` → igual que HELO, pero permite usar extensiones (STARTTLS, AUTH, etc.).  
- `MAIL FROM:<remitente@dominio>` → dirección del remitente.  
- `RCPT TO:<destinatario@dominio>` → dirección del receptor.  
- `DATA` → indica el cuerpo del correo (termina con `.` en una línea).  
- `VRFY <usuario>` → verifica si existe un usuario/buzón en el servidor.  
- `EXPN <lista>` → expande listas de correo (si existen).  
- `RSET` → cancela la transacción actual, pero mantiene la conexión.  
- `QUIT` → cierra la sesión.  

---

## 📌 User Enumeration (`VRFY` y `EXPN`)
- `VRFY` permite preguntar si un usuario existe.  
  - Ejemplo:  
    ```
    VRFY root
    250 2.0.0 root
    ```  
- `EXPN` muestra si una lista de correo existe y quién la compone.  
- Problema: muchos servidores modernos devuelven siempre `252` o `502` para ocultar la información.  

🔧 Herramienta útil:  
```bash
smtp-user-enum -M VRFY -U users.txt -t <IP>
```

NOTA: Algunos servidores tardan en responder por lo que hay que aumentar el QUERY TIMEOUT.

Ejemplo que funcionó:
```bash
smtp-user-enum -M VRFY -U footprinting-wordlist.txt -t 10.129.42.195 -w 25 -v
```
- -w: Para aumentar los segundos del TIMEOUT default:5



---

## 📌 Open Relay
Un **relay** es cuando un servidor reenvía correos.  

- ✅ Configuración correcta: solo deja relays a usuarios autenticados o IPs de confianza.  
- ❌ **Open relay**: acepta correos de cualquiera y los reenvía a cualquier dominio.  

Ejemplo de configuración insegura en Postfix:  
```ini
mynetworks = 0.0.0.0/0
```

Prueba en telnet:  
```bash
MAIL FROM:<fake@evil.com>
RCPT TO:<victima@gmail.com>
```
- Si responde `250 Ok` → 🚨 open relay.  
- Si responde `550 Relaying denied` → protegido.  

Nmap:  
```bash
nmap -p25 --script smtp-open-relay <IP>
```

---

## 📌 Spoofing de correos
Como SMTP no valida el remitente:  
- Se puede falsificar `MAIL FROM:<ceo@empresa.com>` y `From:` en la cabecera.  
- Si el servidor destino no valida **SPF**, **DKIM** o **DMARC**, el correo puede llegar a la bandeja de entrada como si viniera del CEO.  
- En redes internas sin políticas fuertes, esto funciona muy a menudo.  

---

## 📌 Cabeceras de correo
Los **headers** contienen información útil:  
- `From`, `To`, `Subject`, `Date`.  
- `Received:` (muestra los servidores por los que pasó el correo).  
- `Message-ID`, `X-Mailer`.  

Se definen en **RFC5322**.  
👉 Muy útiles en OSINT e investigación de phishing.  

---

## 📌 Seguridad en SMTP
- **STARTTLS** → cifrado tras EHLO.  
- **SMTP-AUTH** → autenticación obligatoria antes de enviar.  
- **SPF** → valida si el servidor está autorizado a enviar en nombre del dominio.  
- **DKIM** → firma digital para validar integridad y autenticidad.  
- **DMARC** → política que indica qué hacer si SPF/DKIM fallan.  

---

## 📌 Herramientas de enumeración SMTP en pentesting
- **Telnet/Netcat** → pruebas manuales.  
- **Nmap** con scripts:
  - `smtp-commands` → lista comandos soportados.  
  - `smtp-open-relay` → testea si es relay abierto.  
  - `smtp-enum-users` → intenta enumerar usuarios.  
- **smtp-user-enum** → fuerza bruta de usuarios vía VRFY/EXPN/RCPT.  

---

✅ **Resumen final para pentester**:  
1. Escanea el puerto 25/587/465.  
2. Identifica banner/version (`nmap -sV`).  
3. Lanza `smtp-commands` para ver capacidades.  
4. Prueba `VRFY` con nombres comunes o wordlist.  
5. Testea open relay (`MAIL FROM` + `RCPT TO` externos).  
6. Analiza cabeceras de correos internos si tienes acceso.  
