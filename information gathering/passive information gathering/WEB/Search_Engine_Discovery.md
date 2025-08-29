# 🔎 Search Engine Discovery (OSINT con buscadores)

## 📌 Definición
Uso de buscadores (Google, Bing, etc.) para **reconocimiento web** y **recopilación de inteligencia de fuentes abiertas (OSINT)**.  
Permite descubrir datos sensibles, páginas ocultas y vulnerabilidades sin interacción directa con el objetivo.

---

## ✅ Ventajas
- **Open Source** → Información pública, legal y ética.  
- **Amplia cobertura** → Motores indexan gran parte de la web.  
- **Simplicidad** → No requiere habilidades técnicas avanzadas.  
- **Gratuito** → Acceso inmediato sin coste.

---

## 📊 Usos principales
- **Security Assessment** → Encontrar vulnerabilidades y datos expuestos.  
- **Competitive Intelligence** → Información de competidores.  
- **Investigative Journalism** → Conexiones ocultas, malas prácticas.  
- **Threat Intelligence** → Identificar actores maliciosos, amenazas emergentes.

⚠️ **Limitaciones**: no todo está indexado (Deep Web, Dark Web, datos protegidos).

---

## 🔐 Search Operators (Google / Bing)

| Operador      | Descripción | Ejemplo | Uso |
|---------------|-------------|---------|-----|
| `site:`       | Limita a un dominio | `site:example.com` | Listar páginas de un dominio |
| `inurl:`      | Palabra en URL | `inurl:login` | Buscar paneles de login |
| `filetype:` / `ext:` | Tipo de fichero | `filetype:pdf` | Buscar documentos descargables |
| `intitle:`    | Palabra en título | `intitle:"confidential report"` | Documentos sensibles |
| `intext:`     | Palabra en cuerpo | `intext:"password reset"` | Formularios, resets |
| `cache:`      | Versión cacheada | `cache:example.com` | Ver contenido previo |
| `link:`       | Páginas que enlazan a otra | `link:example.com` | Backlinks |
| `related:`    | Sitios similares | `related:example.com` | Competencia |
| `info:`       | Resumen info del sitio | `info:example.com` | Datos básicos |
| `define:`     | Definición de palabra | `define:phishing` | Glosario |
| `numrange:` / `..` | Rango numérico | `"price" 100..500` | Productos/precios |
| `allintext:`  | Varias palabras en cuerpo | `allintext:admin password` | Texto sensible |
| `allinurl:`   | Varias palabras en URL | `allinurl:admin panel` | Rutas de admin |
| `allintitle:` | Varias palabras en título | `allintitle:report 2023` | Documentos filtrados |
| `AND / OR / NOT` | Operadores lógicos | `site:bank.com NOT inurl:login` | Excluir páginas |
| `*` (wildcard) | comodín | `user* manual` | user guide, handbook |
| `""` (comillas) | frase exacta | `"security policy"` | Búsqueda exacta |
| `-` | excluir término | `site:news.com -sports` | Filtrar resultados |

---

## 🕵️ Google Dorking
Uso avanzado de operadores para encontrar datos sensibles o vulnerabilidades.  
Ejemplos comunes:

- **Login pages**  
  ```
  site:example.com inurl:login
  site:example.com (inurl:login OR inurl:admin)
  ```
- **Archivos expuestos**  
  ```
  site:example.com filetype:pdf
  site:example.com (filetype:xls OR filetype:docx)
  ```
- **Config files**  
  ```
  site:example.com inurl:config.php
  site:example.com (ext:conf OR ext:cnf)
  ```
- **Database backups**  
  ```
  site:example.com inurl:backup
  site:example.com filetype:sql
  ```

🔗 Más ejemplos: **Google Hacking Database (GHDB)**.
