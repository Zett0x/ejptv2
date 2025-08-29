# robots.txt

## Introducción
- Archivo de texto ubicado en la raíz del sitio:  
  ```
  https://www.example.com/robots.txt
  ```
- Basado en el **Robots Exclusion Standard**.  
- Define reglas (directivas) que indican a los bots **qué pueden y qué no pueden rastrear**.  
- Funciona como una “guía de etiqueta” para crawlers.  

---

## Estructura de robots.txt
Un archivo se compone de **registros**, cada uno con:
1. **User-agent** → especifica el bot (ej. `Googlebot`, `Bingbot`, `*` para todos).  
2. **Directivas** → instrucciones para ese bot.

### Directivas comunes
| Directiva   | Descripción | Ejemplo |
|-------------|-------------|---------|
| `Disallow`  | Bloquea acceso a rutas | `Disallow: /admin/` |
| `Allow`     | Permite acceso, incluso si hay reglas generales de `Disallow` | `Allow: /public/` |
| `Crawl-delay` | Tiempo (segundos) entre peticiones de un bot | `Crawl-delay: 10` |
| `Sitemap`   | URL del sitemap en XML | `Sitemap: https://www.example.com/sitemap.xml` |

---

## Ejemplo de robots.txt
```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /public/

User-agent: Googlebot
Crawl-delay: 10

Sitemap: https://www.example.com/sitemap.xml
```

### Interpretación
- Todos los bots no pueden acceder a `/admin/` y `/private/`.  
- Todos los bots sí pueden acceder a `/public/`.  
- `Googlebot` debe esperar 10 segundos entre peticiones.  
- Hay un `sitemap.xml` publicado para indexación.  

---

## Importancia de robots.txt
- No es obligatorio → bots maliciosos pueden ignorarlo.  
- **Razones para respetarlo**:
  - Evitar sobrecargar el servidor.  
  - Proteger datos sensibles de la indexación.  
  - Cumplir normas legales o de términos de servicio.  

---

## robots.txt en Web Recon
- 🕵️ **Descubrimiento de directorios ocultos**: rutas marcadas como privadas pueden revelar paneles de admin, backups, etc.  
- 🗺️ **Mapeo de la estructura**: inferir áreas del sitio no enlazadas en la navegación pública.  
- 🪤 **Detección de honeypots**: algunos sitios incluyen rutas trampa en robots.txt para detectar bots maliciosos.  
