## Hashcat -m 10900 (PBKDF2-HMAC-SHA256)

Formato esperado: `sha256:ITERACIONES:SALT:HASH`
Ejemplo: `sha256:1000:NjI3MDM3:vVfavLQL9ZWjg8BUMq6/FB8FtpkIGWYk`

Detalles:
- `sha256`: Algoritmo base.
- `1000`: Iteraciones.
- `NjI3MDM3`: Salt (Base64 o Hex).
- `vVfavLQL9ZWjg8BUMq6/FB8FtpkIGWYk`: Hash (Base64 o Hex).
#### Comando para ver ejemplos
```bash
hashcat -m 10900 --example-hashes