
## Introducción
`dig` es una herramienta de línea de comandos muy potente para consultar servidores DNS.  
Sirve para:
- Ver registros de dominios (A, MX, NS, TXT, etc.).  
- Resolver direcciones IP.  
- Hacer *reverse lookups* (PTR).  
- Analizar problemas de DNS.  
- Reconocer infraestructura de un dominio.  

---

## 📜 Lista de comandos útiles

```bash
# Consulta básica (A record)
dig domain.com

# Consulta explícita del registro A (IPv4)
dig domain.com A

# Consulta registro AAAA (IPv6)
dig domain.com AAAA

# Consulta registros de correo (MX)
dig domain.com MX

# Consulta servidores de nombres (NS)
dig domain.com NS

# Consulta registros TXT
dig domain.com TXT

# Consulta registros CNAME
dig domain.com CNAME

# Consulta SOA (Start of Authority)
dig domain.com SOA

# Consulta especificando servidor DNS (ej. Cloudflare)
dig @1.1.1.1 domain.com

# Consulta con traza completa de resolución
dig +trace domain.com

# Reverse lookup (PTR → IP a dominio)
dig -x 134.209.24.248

# Respuesta corta y limpia
dig +short domain.com

# Mostrar solo la sección de respuestas
dig +noall +answer domain.com

# Obtener todos los registros (puede estar limitado por RFC 8482)
dig domain.com ANY
```

---

## 📌 Ejemplos rápidos

```bash
# IP de google.com
dig google.com A

# Dominio asociado a la IP 134.209.24.248
dig -x 134.209.24.248

# Mail servers de facebook
dig facebook.com MX

# Solo IPs en formato limpio
dig +short hackthebox.com
```

---

## ⚠️ Notas importantes
- Muchos servidores bloquean o limitan consultas `ANY`.  
- No abuses de consultas repetitivas → puede ser detectado como escaneo.  
- Combina `dig` con herramientas como `dnsenum`, `dnsrecon` o `theHarvester` para enumeración avanzada.  

---

## Ejemplo práctico (HTB)
- `inlanefreight.com → 134.209.24.248`  
- PTR de `134.209.24.248 → inlanefreight.com`  
- MX de `facebook.com → smtpin.vvv.facebook.com`  
