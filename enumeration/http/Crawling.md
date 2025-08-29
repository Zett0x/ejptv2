# Web Crawling (Creepy Crawlies)

## Introducción
- El **web crawling** consiste en recorrer sitios web de forma automatizada para extraer información.  
- Los **crawlers** o “spiders” permiten descubrir contenido oculto, enlaces, recursos y potenciales vulnerabilidades.  
- Ventaja: automatizan y aceleran el proceso, dejando al analista más tiempo para interpretar datos.  

⚠️ **Buenas prácticas**:  
- Obtener permiso antes de hacer crawling intensivo.  
- Evitar sobrecargar los servidores con demasiadas peticiones.  

---

## Herramientas Populares de Crawling
| Herramienta | Descripción | Características |
|-------------|-------------|-----------------|
| **Burp Suite Spider** | Parte de Burp Suite | Descubre contenido oculto, mapea apps, encuentra vulnerabilidades. |
| **OWASP ZAP Spider** | Gratis y open source | Crawling + escaneo de seguridad, modos manual y automático. |
| **Scrapy** | Framework en Python | Construcción de spiders personalizados, flexible y escalable. |
| **Apache Nutch** | Open source en Java | Crawling masivo, muy extensible, ideal para proyectos grandes. |

---

## Scrapy y ReconSpider

### Instalación de Scrapy
```bash
pip3 install scrapy
```

### Descargar ReconSpider
```bash
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
unzip ReconSpider.zip
```

### Ejecución
```bash
python3 ReconSpider.py http://inlanefreight.com
```

- Reemplazar dominio con el objetivo.  
- Resultados se almacenan en `results.json`.  

---

## Estructura de `results.json`
Ejemplo simplificado:
```json
{
  "emails": ["lily.floid@inlanefreight.com", "cvs@inlanefreight.com"],
  "links": ["https://www.themeansar.com", "https://www.inlanefreight.com/index.php/offices/"],
  "external_files": ["https://www.inlanefreight.com/wp-content/uploads/2020/09/goals.pdf"],
  "js_files": ["https://www.inlanefreight.com/wp-includes/js/jquery/jquery-migrate.min.js?ver=3.3.2"],
  "form_fields": [],
  "images": ["https://www.inlanefreight.com/wp-content/uploads/2021/03/AboutUs_01-1024x810.png"],
  "videos": [],
  "audio": [],
  "comments": ["<!-- #masthead -->"]
}
```

### Datos extraídos
- **emails** → correos encontrados.  
- **links** → enlaces internos/externos.  
- **external_files** → PDFs, documentos, backups.  
- **js_files** → librerías y scripts usados.  
- **form_fields** → formularios en la web.  
- **images/videos/audio** → recursos multimedia.  
- **comments** → comentarios HTML reveladores.  

---

## Valor en Reconocimiento
- Permite crear un **mapa de la aplicación web**.  
- Identifica recursos sensibles como documentos internos.  
- Descubre endpoints y formularios para pruebas posteriores.  
- Exposición de correos → posible ataque de **phishing** o enumeración de usuarios.  
