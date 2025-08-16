# 🛡️ Evasión de Firewalls e IDS/IPS con Nmap

Nmap ofrece múltiples técnicas para evadir **firewalls**, **IDS** e **IPS**, descubriendo servicios que normalmente aparecerían como filtrados o invisibles.

---

## 🔥 Firewalls
- Evalúan tráfico entrante y aplican reglas: **permitir, descartar o rechazar**.
- **Drop (descartar)** → ignora paquetes, sin respuesta.
- **Reject (rechazar)** → responde con `RST` o mensajes ICMP (`Port Unreachable`, `Host Prohibited`, etc.).

---

## 👁️ IDS / IPS
- **IDS** → Detecta patrones sospechosos, avisa al administrador.
- **IPS** → Actúa automáticamente bloqueando tráfico malicioso.
- Estrategia: usar **varias IPs (VPS)** o **decoys** para evadir detección y bloqueo.

---

## ⚡ Técnicas de evasión con Nmap

### 1️⃣ SYN Scan `-sS`
- Escaneo sigiloso con paquetes **SYN**.
- Respuesta:
  - **SYN-ACK** → puerto abierto.
  - **RST** → puerto cerrado.
- Fácil de bloquear por firewall.

```bash
sudo nmap 10.129.2.28 -p 21,22,25 -sS -Pn -n --disable-arp-ping
```

✅ Rápido y fiable.  
❌ Puede ser bloqueado por firewalls/IPS.

---

### 2️⃣ ACK Scan `-sA`
- Envía paquetes con **ACK**.
- El firewall no sabe si la conexión se inició dentro o fuera.
- Permite diferenciar entre:
  - **Unfiltered** → el host responde con `RST`.
  - **Filtered** → no hay respuesta.

```bash
sudo nmap 10.129.2.28 -p 21,22,25 -sA -Pn -n --disable-arp-ping
```

✅ Difícil de filtrar.  
❌ No distingue **open/closed**, solo **filtered/unfiltered**.

---

### 3️⃣ Decoys `-D`
- Inserta **IPs señuelo** en los paquetes enviados.
- Oculta la IP real del atacante.
- Ejemplo con 5 IPs aleatorias:
```bash
sudo nmap 10.129.2.28 -p 80 -sS -Pn -n -D RND:5
```

✅ Oculta el origen real del escaneo.  
❌ Si los señuelos no están activos, puede causar **SYN flood** y bloquear el servicio.

---

### 4️⃣ Spoofing de IP origen `-S`
- Permite falsificar la **IP de origen** de los paquetes.
- Requiere usar `-e` para especificar interfaz.
- Ejemplo:
```bash
sudo nmap 10.129.2.28 -p 445 -O -S 10.129.2.200 -e eth0
```

📌 **Importante**:
- En una **LAN**, el tráfico se entrega realmente por **MAC**.  
- Si hacemos **ARP spoofing** para hacernos pasar por la IP suplantada, podremos recibir las respuestas.  
- En **Internet**, los routers descartan paquetes con IP falsa (**anti-spoofing**).  

✅ Puede evadir reglas de firewall basadas en IP.  
❌ Sin control de la IP suplantada, no recibimos respuestas útiles.

---

### 5️⃣ Source Port Manipulation `--source-port`
- Fuerza a que Nmap use un **puerto específico como origen**.
- Ejemplo: usar puerto 53 (DNS) porque los firewalls suelen permitirlo.
```bash
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --source-port 53
```

✅ Muy efectivo contra firewalls que confían en ciertos puertos.  
❌ Detectable si IPS inspecciona contenido.

---

### 6️⃣ DNS Proxying `--dns-server`
- Permite especificar servidores DNS propios.
- Útil en **DMZ** donde los DNS internos son más confiables.
```bash
sudo nmap 10.129.2.28 --dns-server 192.168.1.53
```

✅ Permite pivotar hacia hosts internos.  
❌ Depende de DNS internos accesibles.

---

## 📊 Cuadro comparativo

| Técnica            | Opción Nmap        | Detección de puertos | Evasión firewall | Contra IPS | Riesgos |
|--------------------|-------------------|----------------------|------------------|------------|---------|
| SYN scan           | `-sS`             | Open / Closed        | Baja             | Baja       | Detectable |
| ACK scan           | `-sA`             | Filtered / Unfiltered| Media            | Media      | No muestra open/closed |
| Decoys             | `-D`              | Normal               | Alta (engaño)    | Media      | Posible SYN flood |
| IP spoofing        | `-S`              | Limitado             | Alta             | Alta       | No hay respuesta si no controlas la IP |
| Source port        | `--source-port`   | Open / Closed        | Alta             | Alta       | Puede alertar IPS |
| DNS proxying       | `--dns-server`    | Nombres resueltos    | Media            | Baja       | Depende de DNS internos |

---

## 🛠️ Conexión manual a puertos filtrados
Si descubrimos que un puerto filtrado responde usando un **puerto de origen confiable**, podemos conectarnos manualmente:

```bash
ncat -nv --source-port 53 10.129.2.28 50000
```

Ejemplo: acceso a un servicio **ProFTPd** en puerto 50000 gracias a que el firewall permite tráfico desde el puerto 53.

---
