# 🌐 Domain information

La información de dominios es **clave en la enumeración inicial**, ya que nos muestra la **presencia en Internet** de la empresa:  
- Sitios web principales y subdominios.  
- Proveedores de servicios externos.  
- Tecnologías y configuraciones utilizadas.  
- Posibles vectores de ataque (APIs, acceso remoto, servicios mal configurados).  

👉 Se recopila **de forma pasiva (OSINT)**, evitando escaneos directos que delaten al tester.

---

## 🔎 Metodología de recopilación

### 1. **Análisis de la web principal**
- Observar textos, secciones y servicios ofrecidos.  
- Identificar tecnologías necesarias: hosting, IoT, desarrollo de apps, seguridad, etc.  
- Aplicar los **principios de enumeración**:  
  - Lo que vemos → página, servicios visibles.  
  - Lo que no vemos → infraestructura y funcionamiento interno.  

---

### 2. **Certificados SSL**
Los certificados TLS/SSL de un dominio suelen contener **múltiples FQDNs** (dominios y subdominios).  
Ejemplo:  
```
inlanefreight.htb
www.inlanefreight.htb
support.inlanefreight.htb
```

✅ Útil porque:  
- Revela subdominios ocultos.  
- Permite correlacionar infraestructura activa.  

---

### 3. **Certificate Transparency (crt.sh)**
Los certificados emitidos deben registrarse en **logs públicos** (RFC 6962).  
📌 crt.sh es un buscador de esos registros.  

- Consulta básica:  
```bash
https://crt.sh/?q=inlanefreight.com
```

- En JSON (para automatizar):  
```bash
curl -s "https://crt.sh/?q=inlanefreight.com&output=json" | jq .
```

- Filtrar solo subdominios únicos:  
```bash
curl -s "https://crt.sh/?q=inlanefreight.com&output=json" \
| jq -r '.[].name_value' \
| tr ' ' '\n' \
| sed 's/\*\.//g' \
| sort -u
```

👉 Esto nos da un **mapa de subdominios**, incluso históricos.  

---

### 4. **Resolución DNS + Hosts propios**
Una vez con la lista de subdominios, podemos verificar **qué servidores están gestionados directamente por la empresa** y cuáles por terceros.  

```bash
for i in $(cat subdomainlist); do 
    host "$i" | awk '/has address/{print $1, $4}'
done
```

Ejemplo resultado:  
```
blog.inlanefreight.com   10.129.24.93
www.inlanefreight.com    10.129.127.33
```

✅ Los IPs en el mismo rango suelen indicar **infraestructura propia**.  
❌ Un dominio que apunte a AWS, Cloudflare u otro proveedor → fuera de alcance salvo permiso.  

---

### 5. **Shodan**
Permite **enriquecer la información de las IPs** detectadas, mostrando:  
- Puertos abiertos.  
- Versiones de servicios.  
- Geolocalización.  
- Organización.  

Ejemplo:  
```bash
for ip in $(cat ip-addresses.txt); do 
    shodan host "$ip"
done
```

Resultado (resumido):  
```
10.129.24.93 → nginx (HTTP/HTTPS)
10.129.27.33 → OpenSSH + nginx
10.129.127.22 → Apache (HTTP/HTTPS) + SMTP + DNS + POP3
```

✅ Aquí ya podemos marcar posibles vectores:  
- Webapps vulnerables en Apache/nginx.  
- Servicios de correo (SMTP/POP3).  
- Servidores DNS expuestos.  

---

### 6. **Registros DNS**
Un `dig any` revela varios tipos de registros:  

```bash
dig any inlanefreight.com
```

Ejemplo hallazgos:  
- **A records** → IPs asociadas a dominios (`10.129.27.33`).  
- **MX records** → servidores de correo (Google).  
- **NS records** → servidores de nombres (INWX).  
- **TXT records** → claves y configuraciones de terceros.  

TXT records analizados:  
```
atlassian-domain-verification=...
google-site-verification=...
logmein-verification-code=...
v=spf1 include:mailgun.org include:_spf.google.com include:spf.protection.outlook.com ...
```

✅ Información crítica:  
- **Atlassian** → usan Jira/Confluence → posible ataque vía credenciales filtradas.  
- **Google / Outlook / Office 365** → email corporativo y documentos en la nube.  
- **LogMeIn** → control remoto centralizado (riesgo alto si comprometen credenciales).  
- **Mailgun** → APIs de correo, posibles vulnerabilidades en endpoints.  
- **INWX** → proveedor de dominios.  

---

## 🎯 Conclusiones
- **Dominios y subdominios** → revelan superficie de ataque.  
- **IPs** → separan servicios propios de los de terceros.  
- **Shodan** → confirma servicios expuestos.  
- **Registros DNS** → revelan proveedores y tecnologías clave.  
- **Servicios externos** (Atlassian, LogMeIn, O365, Mailgun) → puntos de ataque indirectos.  

👉 Todo esto **sin escanear activamente**, solo con OSINT y consultas pasivas.  

---

# 📑 Chuleta de Comandos de Enumeración de Dominios

## ✅ DNS básicos
```bash
host dominio.com
dig A dominio.com +short
dig MX dominio.com +short
dig TXT dominio.com +short
dig NS dominio.com +short
```

## 🪪 WHOIS
```bash
whois dominio.com
```

## 🔐 Certificate Transparency
```bash
curl -s "https://crt.sh/?q=dominio.com&output=json" | jq .
```

## 🛰️ Shodan
```bash
shodan init TU_API_KEY
shodan host 1.2.3.4
shodan search --fields ip_str,port,product "hostname:dominio.com"
```

## 📦 Resolución masiva
```bash
# Subdominios → IPs
while read -r s; do host "$s" | awk '/has address/{print $1, $4}'; done < subdomainlist

# Solo IPs únicas
while read -r s; do host "$s" | awk '/has address/{print $4}'; done < subdomainlist | sort -u > ip-addresses.txt

# IPs → Shodan
while read -r ip; do shodan host "$ip"; done < ip-addresses.txt
```

## 🧪 Transferencia de zona (siempre con permiso)
```bash
for ns in $(dig NS dominio.com +short); do dig AXFR dominio.com @$ns; done
```

---

## 📘 Tipos de registros DNS más útiles

| Tipo | Significado | Utilidad práctica |
|------|-------------|------------------|
| **A / AAAA** | IP de un dominio (IPv4/IPv6) | Mapeo directo de infraestructura |
| **CNAME** | Alias hacia otro dominio | Identificar uso de proveedores externos (CDNs, SaaS) |
| **MX** | Servidores de correo | Saber si usan Google, O365, self-hosted |
| **NS** | Servidores de nombres | Identificar proveedor DNS y posible intento de AXFR |
| **TXT** | Verificación y políticas | SPF, DKIM, DMARC, claves de validación de SaaS |
| **SOA** | Info de zona | Muestra contacto admin, serial y tiempos |

---
