# ☁️ Recursos en la Nube – Cloud Resources

Las empresas modernas dependen de **AWS, GCP y Azure** para centralizar gestión y permitir trabajo remoto.  
Aunque los proveedores aseguran la infraestructura, las **configuraciones de administradores** pueden dejar recursos expuestos.

---

## 🔎 Riesgos comunes
- **AWS S3 buckets**, **Azure Blobs**, **GCP Cloud Storage** → accesibles **sin autenticación** si están mal configurados.  
- Pueden aparecer en **registros DNS** para que empleados accedan fácilmente, lo que facilita su descubrimiento.

Ejemplo:
```
s3-website-us-west-2.amazonaws.com  10.129.95.250
```

---

## 🛠️ Métodos de descubrimiento

### 1. **Google Dorks**
- Buscar recursos públicos:
```text
intext:<empresa> inurl:amazonaws.com
intext:<empresa> inurl:blob.core.windows.net
intext:<empresa> inurl:storage.googleapis.com
```
👉 Resultados: PDFs, DOCX, ZIP, código fuente.

### 2. **Código fuente de la web**
- Revisar `<link>`, `<script>`, `<img>` en HTML → a menudo cargan contenido desde buckets/blobs:
```html
<link rel="dns-prefetch" href="https://<empresa>.blob.core.windows.net">
<script src="https://<empresa>.s3.amazonaws.com/app.js"></script>
```

### 3. **Servicios de terceros**
- **domain.glass** → muestra infra, SSL, Cloudflare y seguridad.  
- **GrayHatWarfare** → buscador de buckets en AWS/Azure/GCP.  
  - Permite filtrar por **formato de archivo**.  
  - Ejemplo: buscar `company` en GrayHatWarfare y filtrar por `.pdf`.

### 4. **Convenciones de nombres**
- Muchas empresas usan abreviaciones (`acme-dev`, `acme-prod`).  
- Buscar variaciones en combinación con Dorks o GrayHatWarfare.  

---

## ⚠️ Ejemplos de hallazgos críticos
- **SSH keys expuestas** (`id_rsa`, `id_rsa.pub`) → acceso directo a servidores.  
- **Backups completos** con credenciales.  
- **Archivos internos** (código, presentaciones, datos sensibles).  

---

## 🧑‍💻 Comandos útiles para descubrir recursos cloud

### 1. Resolución DNS (buscar referencias a cloud en subdominios)
```bash
for i in $(cat subdomainlist); do host $i; done | grep amazonaws
for i in $(cat subdomainlist); do host $i; done | grep blob.core.windows.net
for i in $(cat subdomainlist); do host $i; done | grep storage.googleapis.com
```

### 2. Detectar buckets desde Shodan (por hostname)
```bash
shodan search 'hostname:amazonaws.com org:"<empresa>"'
shodan search 'hostname:blob.core.windows.net org:"<empresa>"'
```

### 3. Detectar buckets expuestos vía dig/nslookup
```bash
dig CNAME static.<empresa>.com +short
dig TXT <empresa>.com +short | grep amazonaws
```

### 4. Google Dorks (ejemplos prácticos)
```text
site:amazonaws.com intext:<empresa>
site:blob.core.windows.net intext:<empresa>
site:storage.googleapis.com intext:<empresa>
```

---

## 🎯 Conclusiones
- La nube añade **superficie de ataque extra**.  
- Revisar **DNS, Dorks, código fuente, GrayHatWarfare y servicios de terceros**.  
- Un bucket mal configurado = fuga de datos críticos o acceso a servidores.

---
