# Automating Recon

## ¿Por qué automatizar el reconocimiento?
Automatizar tareas de recon en lugar de hacerlas manualmente aporta:

- **Eficiencia** → más rápido que los humanos, libera tiempo para el análisis.  
- **Escalabilidad** → permite abarcar muchos dominios/objetivos.  
- **Consistencia** → menos errores humanos, resultados reproducibles.  
- **Cobertura completa** → DNS, subdominios, crawling, escaneo de puertos, etc.  
- **Integración** → se conecta con otros frameworks y herramientas para un flujo continuo.  

---

## Frameworks de Recon
- **FinalRecon**  
  - Python, modular.  
  - SSL, Whois, headers, crawling, DNS, subdominios, directorios, Wayback, escaneo de puertos.  

- **Recon-ng**  
  - Framework en Python.  
  - DNS, subdominios, puertos, crawling, incluso exploits.  

- **theHarvester**  
  - Emails, subdominios, hosts, nombres de empleados, banners.  
  - Fuentes: buscadores, PGP, Shodan.  

- **SpiderFoot**  
  - OSINT automation.  
  - Integra múltiples fuentes: IPs, dominios, emails, redes sociales.  
  - Soporta crawling, DNS, puertos.  

- **OSINT Framework**  
  - Colección de recursos y herramientas de OSINT (social media, buscadores, registros públicos, etc.).  

---

## FinalRecon – Funcionalidades
- **Header Information** → servidor, tecnologías, configuraciones erróneas.  
- **Whois Lookup** → datos de registro de dominio.  
- **SSL Info** → validez, emisor, detalles.  
- **Crawler** → analiza HTML, JS, CSS, links internos/externos, imágenes, robots.txt, sitemap.xml, links ocultos, Wayback.  
- **DNS Enumeration** → más de 40 tipos de registros (incluido DMARC).  
- **Subdomain Enumeration** → crt.sh, ThreatMiner, Shodan, VirusTotal, etc.  
- **Directory Enumeration** → con wordlists personalizadas.  
- **Wayback Machine** → URLs de los últimos 5 años.  
- **Port Scan (fast)**.  

---

## Instalación de FinalRecon
```bash
git clone https://github.com/thewhiteh4t/FinalRecon.git
cd FinalRecon
pip3 install -r requirements.txt
chmod +x ./finalrecon.py
./finalrecon.py --help
```

---

## Uso – Opciones principales
```
--url URL      Especificar el objetivo
--headers      Obtener headers
--sslinfo      Info SSL
--whois        Lookup Whois
--crawl        Crawling del sitio
--dns          DNS enumeration
--sub          Subdomain enumeration
--dir          Directory search
--wayback      URLs históricas
--ps           Fast port scan
--full         Recon completo
```

### Ejemplo
```bash
./finalrecon.py --headers --whois --url http://inlanefreight.com
```

Salida: muestra **headers HTTP** + **Whois** del dominio.  
