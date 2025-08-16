# 👥 Staff – Información sobre Empleados

La identificación de empleados en redes sociales y fuentes públicas revela datos clave sobre la **infraestructura tecnológica** de una empresa y sus procesos internos.

---

## 🔎 Fuentes principales
- **Redes profesionales**: LinkedIn, Xing → perfiles, puestos, proyectos, habilidades.  
- **Ofertas de empleo**: muestran tecnologías, frameworks, bases de datos, metodologías.  
- **Repositorios públicos**: GitHub, GitLab, Bitbucket → código, configuraciones, posibles fugas de credenciales.  

---

## 📌 Información extraíble

### 1. Lenguajes y tecnologías (ejemplo de oferta de empleo)
- **Lenguajes**: Java, C#, C++, Python, Ruby, PHP, Perl.  
- **Bases de datos**: PostgreSQL, MySQL, Oracle.  
- **Frameworks web**: Flask, Django, ASP.NET, Spring.  
- **Control de versiones**: Git, SVN, Perforce.  
- **Otros**: Docker, Kubernetes, Redis, NumPy, Atlassian Suite.  

👉 Esto ayuda a perfilar la **stack tecnológica interna**.

### 2. Seguridad y procesos
- Certificaciones requeridas (ej. Security+) → nivel de madurez en seguridad.  
- **Agile, CI/CD** → metodologías de desarrollo.  
- **Atlassian Suite** (Jira, Confluence, Bitbucket) → posibles puntos de entrada.  

### 3. Perfiles de empleados
- Descripción de **proyectos** y **tecnologías usadas**.  
- Enlaces a **repositorios open source** → riesgo de fugas (emails, tokens, claves).  
- Ejemplo: código público con un **JWT hardcodeado** o emails personales.  

---

## ⚠️ Riesgos
- **Exposición involuntaria** de información técnica sensible.  
- **Errores humanos en repositorios** → claves o contraseñas en código.  
- **Sobreexposición en redes sociales** → facilita spear phishing e ingeniería social.  

---

## 🧑‍💻 Técnicas y Dorks útiles

### 🔍 Google Dorks para LinkedIn
```text
site:linkedin.com/in "CompanyName"
site:linkedin.com/in "CompanyName" AND developer
site:linkedin.com/jobs "CompanyName" AND security
```

### 🔍 Google Dorks para GitHub
```text
site:github.com "CompanyName"
site:github.com "CompanyName" password
site:github.com "CompanyName" jwt
site:github.com "CompanyName" key
```

### 🔍 Google Dorks para Job Posts
```text
site:indeed.com "CompanyName" developer
site:glassdoor.com "CompanyName" Python
site:linkedin.com/jobs "CompanyName" SQL
```

### 🔍 Ejemplo práctico en LinkedIn
- Buscar: `"CompanyName" site:linkedin.com/in`  
- Filtrar resultados por **desarrolladores** o **seguridad**.  
- Revisar habilidades y tecnologías listadas → stack interna.

### 🔍 Ejemplo práctico en GitHub
- Buscar: `"CompanyName" site:github.com`  
- Revisar repositorios personales de empleados que mencionen la empresa.  
- Posibles hallazgos: emails, claves API, configuraciones con credenciales.  

---

## 🎯 Conclusión
Analizar perfiles y repositorios permite:  
- Conocer la **stack tecnológica real**.  
- Identificar **medidas de seguridad aplicadas**.  
- Detectar **fugas de información sensibles**.  

👉 Los empleados técnicos (devs + seguridad) son los más relevantes para esta fase.

---
