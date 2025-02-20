# 📡 SNMP (Simple Network Management Protocol)

## 🔎 ¿Qué es SNMP?
SNMP es un protocolo de red utilizado para **monitorear y gestionar dispositivos** como routers, switches, servidores, impresoras y más.

## 📌 Funcionamiento
SNMP sigue un modelo **cliente-servidor**:
1. **Agente SNMP** en el dispositivo recopila información del sistema.
2. **Servidor NMS (Network Management System)** consulta datos o recibe alertas.
3. **Los datos se almacenan en una base llamada MIB** (Management Information Base).

## 📌 Versiones de SNMP
| **Versión**  | **Características** |
|-------------|----------------------|
| **SNMPv1**  | Inseguro, credenciales en texto plano. |
| **SNMPv2c** | Mejor rendimiento, pero sigue sin cifrado. |
| **SNMPv3**  | Cifrado y autenticación segura. ✅ |

⚠️ **SNMPv1 y SNMPv2c son vulnerables y aún se usan en muchas empresas.**

## 📌 Puertos utilizados
- **UDP 161** → Para consultas SNMP (`GET`, `SET`, `WALK`).
- **UDP 162** → Para recibir alertas SNMP (`TRAP`, `INFORM`).

## 📌 Comandos SNMP básicos
```bash
# Obtener información de un dispositivo
snmpget -v2c -c public 10.2.19.36 1.3.6.1.2.1.1.5.0

# Recorrer toda la MIB del dispositivo
snmpwalk -v2c -c public 10.2.19.36

# Modificar valores en el dispositivo (si se tiene acceso)
snmpset -v2c -c private 10.2.19.36 1.3.6.1.2.1.1.5.0 s "NuevoNombre"


# 🔑 SNMP Community Strings

## 📌 ¿Qué es una "Community String" en SNMP?
La **Community String** es una **contraseña en texto plano** utilizada en **SNMPv1 y SNMPv2c** para autenticar solicitudes entre el **servidor NMS** (Network Management System) y los **dispositivos gestionados**.

📌 **SNMPv3 no usa Community Strings**, ya que introduce autenticación y cifrado más seguros.

---

## 📌 Tipos de Community Strings
| **Tipo**   | **Descripción** |
|-----------|---------------|
| **`public`**  | Permite solo **lectura** (`GET`, `WALK`). ⚠️ **Insegura** (por defecto en muchos dispositivos). |
| **`private`** | Permite **lectura y escritura** (`SET`). ⚠️ **Riesgosa**, puede modificar configuración del dispositivo. |

---

## 📌 Cómo encontrar una Community String
Si SNMP está mal configurado, puedes descubrir la cadena con herramientas como:
```bash
# Enumerar SNMP usando una lista de posibles Community Strings
onesixtyone -c community_list.txt -i targets.txt

# Escanear un objetivo para SNMP
snmpwalk -v2c -c public 10.2.19.36

# Intentar modificar configuración (requiere una comunidad con permisos de escritura)
snmpset -v2c -c private 10.2.19.36 1.3.6.1.2.1.1.5.0 s "NuevoNombre"
