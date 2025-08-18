# 🌐 DNS – Resumen de Footprinting

El **Domain Name System (DNS)** traduce nombres de dominio en direcciones IP.  
Es como una guía telefónica distribuida: no tiene una base central, sino miles de servidores repartidos globalmente.

---

## 📌 Tipos de servidores DNS
- **Root Server** → Punto de partida, responsables de TLDs (.com, .org, .net). Existen 13 principales coordinados por ICANN.  
- **Autoritativo** → Responde con autoridad sobre un dominio concreto (sus respuestas son vinculantes).  
- **No autoritativo** → No tienen autoridad sobre el dominio, pero responden usando consultas a otros servidores.  
- **Caching** → Guardan en caché respuestas durante el tiempo que marca el TTL.  
- **Forwarding** → Solo reenvían consultas a otros servidores.  
- **Resolver** → Componente local (PC o router) que hace la resolución.

🔐 **Seguridad**: El DNS viaja en claro → se puede espiar/manipular.  
Mitigación: **DoT (DNS over TLS)**, **DoH (DNS over HTTPS)** y **DNSCrypt**.

---

## 📌 Tipos de registros DNS
- **A** → Devuelve una IPv4.  
- **AAAA** → Devuelve una IPv6.  
- **MX** → Servidores de correo.  
- **NS** → Servidores de nombres.  
- **TXT** → Información variada (SPF, DMARC, validaciones de Google/SSL).  
- **CNAME** → Alias de otro dominio.  
- **PTR** → Búsqueda inversa (IP → dominio).  
- **SOA** → Información de la zona (responsable, parámetros de refresco, contacto admin).

---

## 📌 Consultas recursivas vs iterativas

### 🔹 Recursiva
El **servidor DNS hace todo el trabajo por el cliente** hasta conseguir la respuesta final.
1. Cliente pregunta: *“¿Cuál es la IP de www.ejemplo.com?”*.  
2. Servidor recursivo → si no sabe la respuesta, pregunta a los **Root Servers**.  
3. Los Root indican qué servidor maneja el TLD (.com).  
4. El servidor recursivo consulta al servidor .com → este indica el servidor autoritativo de `ejemplo.com`.  
5. El recursivo finalmente pregunta al autoritativo, obtiene la IP y se la devuelve al cliente.

👉 El cliente recibe la **respuesta completa en un único paso**.  
👉 Ejemplo: servidores DNS de tu ISP o de Google (8.8.8.8).

### 🔹 Iterativa
El **cliente va resolviendo paso a paso**.  
1. Pregunta al Root → este responde: *“No lo sé, pero consulta al TLD .com”*.  
2. Pregunta al servidor .com → responde: *“Pregunta al autoritativo de ejemplo.com”*.  
3. Pregunta al autoritativo → obtiene la IP.

👉 El cliente debe hacer las consultas sucesivas.  

### 🚨 Riesgo en seguridad
Si un DNS permite **consultas recursivas abiertas**:
- Puede usarse para **cache poisoning** (envenenar entradas de caché).  
- Se abusa en ataques **DDoS por amplificación DNS**.  

---

## 📌 Archivos y configuración en Bind9
- **named.conf.local / named.conf.options / named.conf.log** → Configuración global y de zonas.  
- **Zone files** → Ficheros con registros (A, MX, NS, etc.).  
- **Reverse lookup files** → Contienen registros PTR para resolución inversa.  

Configuraciones peligrosas:
- **allow-query any;** → Cualquiera puede consultar.  
- **allow-recursion any;** → Resuelve recursivamente para cualquiera.  
- **allow-transfer any;** → Cualquiera puede pedir **transferencia de zona** (AXFR).  

---

## 🕵️ Footprinting y Enumeración DNS

### 🔹 Consultas útiles con `dig`
- **Servidores NS**  
  ```bash
  dig ns inlanefreight.htb @10.129.14.128
  ```
- **Versión de BIND** (si está expuesta)  
  ```bash
  dig CH TXT version.bind @10.129.120.85
  ```
- **Todos los registros visibles (ANY)**  
  ```bash
  dig any inlanefreight.htb @10.129.14.128
  ```

### 🔹 Zone Transfer (AXFR)
Permite clonar la **zona DNS completa** si está mal configurado:
```bash
dig axfr inlanefreight.htb @10.129.14.128
```
👉 Filtra **hosts internos, IPs y servicios críticos**.

### 🔹 Fuerza bruta de subdominios
- Script en Bash:  
  ```bash
  for sub in $(cat subdomains.txt); do 
      dig $sub.inlanefreight.htb @10.129.14.128 +short
  done
  ```
- Con **dnsenum**:  
  ```bash
  dnsenum --dnsserver 10.129.14.128 \
          -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt \
          inlanefreight.htb
  ```

---

## 📌 Claves para un pentester
- Enumerar registros básicos (A, MX, NS, TXT, SOA).  
- Intentar **zone transfer (AXFR)**.  
- Brute force de subdominios → descubrir hosts internos (`internal`, `dc1`, `vpn`, etc.).  
- Consultar versión de BIND → buscar CVEs.  
- Revisar registros TXT (SPF/DMARC) → revelan integraciones externas.  

---

✅ **En resumen**:  
El DNS no solo resuelve nombres, también es una **fuente de inteligencia crítica**.  
Una mala configuración como permitir **AXFR** equivale a entregar un mapa completo de la red al atacante.
