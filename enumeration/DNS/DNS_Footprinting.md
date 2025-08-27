# 🌐 DNS – Footprinting

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

### 📝 Nota rápida sobre AXFR

- Una **transferencia de zona (AXFR)** muestra los registros de un dominio/subdominio.
    
- **Si la transferencia de zona de un subdominio no devuelve nada → NO significa que no existan más subdominios.**
    
- Esos hosts pueden descubrirse con **fuerza bruta de subdominios** usando diferentes wordlists.
    
#### Ejemplo

1. Transferencia de zona de `inlanefreight.htb` devuelve varios subdominios, entre ellos `internal.inlanefreight.htb`.
    
2. Hacemos AXFR de `internal.inlanefreight.htb` → aparecen más hosts como `dc1.internal.inlanefreight.htb`.
    
3. También vemos `dev.inlanefreight.htb`. Su AXFR no devuelve nada, pero con fuerza bruta (ej: `dnsenum`) descubrimos:
    

`dev1.dev.inlanefreight.htb.   604800 IN A 10.12.3.6 ns.dev.inlanefreight.htb.     604800 IN A 127.0.0.1 vpnx.dev.inlanefreight.htb.   604800 IN A 10.12.1.254 win2k.dev.inlanefreight.htb.  604800 IN A 10.12.3.203`

✅ El host **`win2k.dev.inlanefreight.htb`** (10.12.3.203) era justo el que buscábamos. (en el assessment de htb)

**NOTA:** probar distintos wordlists. En este caso usé:  
`/usr/share/seclists/Discovery/DNS/namelist.txt`

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

De subdominios sin indicar servidor dns, por lo que buscará de forma público:
```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -r
```

- `-r`: This option enables recursive subdomain brute-forcing, meaning that if `dnsenum` finds a subdomain, it will then try to enumerate subdomains of that subdomain.
-  `-f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt`: We indicate the path to the SecLists wordlist we'll use for brute-forcing. Adjust the path if your SecLists installation is different.

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

