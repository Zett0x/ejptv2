# Web Enumeration Tools Summary

## WhatWeb
```bash
whatweb TARGET
```
- Identifica **tecnologías** usadas por un sitio web: servidor, CMS, frameworks, emails, librerías JS.  
- Puede ejecutarse sobre **un host** o **un rango de red**.  
- Útil para detectar posibles vulnerabilidades específicas del stack tecnológico.

**Ejemplos:**
```bash
whatweb 10.10.10.121
whatweb --no-errors 10.10.10.0/24
```

---

## robots.txt
- Archivo en la raíz de un sitio (`/robots.txt`) que indica a buscadores qué rutas **no deben indexar**.  
- Suele revelar directorios o páginas sensibles como `/admin`, `/private`, `/backup`.  
- Basta con acceder vía navegador o `curl`:
```bash
curl http://TARGET/robots.txt
```

**Ejemplo de contenido:**
```text
User-agent: *
Disallow: /private
Disallow: /uploaded_files
```
- Muchas veces estas rutas son accesibles directamente y pueden contener paneles de administración o archivos confidenciales.

---

## cURL (Banner Grabbing)
```bash
curl -IL https://DOMAIN
```
- Recupera **cabeceras HTTP** para identificar el servidor web, frameworks, opciones de autenticación y configuraciones.  
- Útil para obtener información rápida sin navegador.  

**Ejemplo de salida:**
```text
HTTP/1.1 200 OK
Server: Apache/2.4.29 (Ubuntu)
Content-Type: text/html; charset=UTF-8
```

---

## SSL/TLS Certificate Inspection
- Si el sitio usa **HTTPS**, inspeccionar el certificado puede revelar emails, nombre de empresa, ubicación y otra metadata útil.  
- Se puede hacer desde el navegador (Ver detalles del certificado) o usando `openssl`:
```bash
openssl s_client -connect TARGET:443 -showcerts
```
- Útil para reconocimiento pasivo y posibles ataques de phishing si el email está dentro del alcance del pentest.
