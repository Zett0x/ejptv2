# WHOIS – Uso en Ciberseguridad

## ¿Qué es WHOIS?
Es un servicio que permite obtener información sobre un dominio de Internet:  
- Fecha de creación, actualización y expiración.  
- Registrador (la empresa donde se compró el dominio).  
- Contactos (registrante, administrador, técnico).  
- Servidores de nombres (Name Servers).  
- Estado del dominio (prohibiciones de borrado, transferencia o actualización).  

Se utiliza mucho en **OSINT y ciberseguridad** para investigar dominios relacionados con phishing, malware o amenazas.

---

## Escenarios prácticos de uso

### 1. Phishing Investigation
- El analista recibe un email sospechoso.  
- Realiza un WHOIS sobre el dominio de la URL.  
- Indicadores de phishing:  
  - Dominio registrado hace pocos días.  
  - Datos del registrante ocultos.  
  - Name servers asociados a *bulletproof hosting*.  

👉 Conclusión: dominio malicioso → se bloquea y se alerta a empleados.

---

### 2. Malware Analysis
- Malware comunica con un **C2 server**.  
- El investigador analiza el dominio vía WHOIS.  
- Hallazgos típicos:  
  - Registrante con email gratuito/anónimo.  
  - Dirección en país con alta actividad de ciberdelitos.  
  - Registrador permisivo con abusos.  

👉 Conclusión: infraestructura sospechosa → se reporta al proveedor de hosting.

---

### 3. Threat Intelligence Report
- Una empresa de ciberseguridad estudia a un **grupo APT** (*Advanced Persistent Threat*: grupos organizados y persistentes, normalmente patrocinados por estados o con gran capacidad técnica).  
- Analiza varios dominios vía WHOIS.  
- Patrones encontrados:  
  - Dominios registrados en clústeres temporales.  
  - Alias e identidades falsas.  
  - Uso repetido de los mismos name servers.  
  - Historial de *takedown*.  

👉 Conclusión: elaboración de **TTPs** (*Tactics, Techniques, and Procedures*: tácticas, técnicas y procedimientos que describen cómo actúa un atacante) e **IOCs** (*Indicators of Compromise*: indicadores técnicos de compromiso como IPs, hashes, dominios sospechosos, etc.) que se comparten con otras organizaciones.

---

## Instalación y uso en Linux
```bash
sudo apt update
sudo apt install whois -y
```

### Ejemplo de uso
```bash
whois facebook.com
```

**Salida destacada:**  
- Registrar: RegistrarSafe, LLC  
- Creation Date: 1997-03-29  
- Expiry Date: 2033-03-30  
- Registrant: Meta Platforms, Inc.  
- Name Servers: A.NS.FACEBOOK.COM, B.NS.FACEBOOK.COM…  
- Domain Status: protegido contra transferencias, borrados y cambios.  

---

## Casos de preguntas tipo examen (HTB)
- WHOIS de **paypal.com** → el IANA ID del registrador es **292**.  
- WHOIS de **tesla.com** → el email de contacto admin es **admin@dnstinations.com**.  
