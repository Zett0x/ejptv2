# 📡 SNMP

## 🔹 ¿Qué es?
- **Simple Network Management Protocol (SNMP)**: protocolo estándar para **monitorizar y administrar dispositivos de red y servidores** de forma remota.  
- Presente en **routers, switches, servidores, impresoras, IoT**, etc.  
- Usa **UDP 161** (consultas) y **UDP 162** (*traps*: notificaciones enviadas por el agente al gestor).  
- Versión actual: **SNMPv3** (más seguro, añade autenticación y cifrado).  

---

## 📂 Conceptos Clave

### MIB (Management Information Base)
- Fichero de texto en formato **ASN.1**.  
- Define la jerarquía de objetos accesibles (OIDs).  
- No contiene datos, solo la estructura y descripciones.

### OID (Object Identifier)
- Identificador jerárquico único (ejemplo: `.1.3.6.1.2.1.1.4.0`).  
- Cada nodo describe un objeto SNMP (ej: `sysContact` = email del admin).  
- Cuanto más larga la cadena, más específica la info.

### Community Strings
- Funcionan como “contraseñas” para acceder a SNMP.  
- **Por defecto**:  
  - `public` → lectura (ro).  
  - `private` → lectura/escritura (rw).  
- **Problema**: en v1 y v2c viajan en **texto plano** → fáciles de interceptar o brute-forcear.

---

## 🔑 Versiones

- **SNMPv1**: sin autenticación ni cifrado.  
- **SNMPv2c**: añade mejoras, pero mantiene inseguridad (community strings en claro).  
- **SNMPv3**: seguridad real → autenticación con usuario/contraseña y cifrado.  

---

## ⚙️ Arquitectura
- Cada servidor/dispositivo corre un **agente SNMP** (`snmpd`).  
- Un **gestor SNMP** central (ej. Zabbix, Nagios, Cacti, PRTG) consulta y recibe datos de muchos agentes.  
- Así se puede:  
  - Monitorizar el estado de toda la red desde un solo punto.  
  - Configurar remotamente parámetros de los dispositivos.  

---

## 📌 Ejemplo de Datos que se Pueden Obtener
- **Identificación básica** (MIB-II):
  - `sysDescr` → descripción del SO/kernel.  
  - `sysContact` → email del admin (ej: `mrb3n@inlanefreight.htb`).  
  - `sysLocation` → ubicación física.  
  - `sysName` → nombre del host.  
  - `sysUpTime` → tiempo de actividad.  

- **Recursos del sistema**:
  - Interfaces de red (estado, tráfico).  
  - Uso de CPU y RAM.  
  - Procesos en ejecución.  
  - Paquetes instalados.  

- **Configuración avanzada** (si está habilitado modo escritura):
  - Activar/desactivar interfaces.  
  - Modificar tablas de enrutamiento.  
  - Cambiar `sysContact`, `sysName`, `sysLocation`.  
  - Configurar traps.  
  - En algunos casos, matar procesos o reiniciar servicios.  

---

## ⚠️ Riesgos Comunes
- **Exposición de información sensible**: emails de admins, versiones de software, lista de paquetes.  
- **DoS o manipulación** si hay permisos de escritura (`rwcommunity`).  
- **Community strings por defecto** (`public`/`private`) muy comunes.  

---

## 🛠️ Herramientas de Footprinting SNMP

### 🔍 snmpwalk
Consultar OIDs recursivamente:
```bash
snmpwalk -v2c -c public 10.129.14.128
```

### 🔓 onesixtyone
Brute-force de *community strings*:
```bash
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp.txt 10.129.14.128
```

### 🗂 braa
Brute-force de OIDs con una community string conocida:
```bash
braa public@10.129.14.128:.1.3.6.*
```

---

## 🧪 Ejemplo Real (HTB)
Salida de `snmpwalk`:
```
sysDescr   → "Linux htb 5.11.0-34-generic ..."
sysContact → "mrb3n@inlanefreight.htb"
sysName    → "htb"
sysLocation→ "Sitting on the Dock of the Bay"
Paquetes   → proftpd, python3, pulseaudio, etc.
```

📌 Con esto un atacante puede:
- Identificar **el SO y kernel exacto** → buscar CVEs.  
- Conseguir el **email real del admin** → phishing o fuerza bruta.  
- Ver **software instalado** → detectar servicios vulnerables.  
- Mapear usuarios internos (`mrb3n`).  
