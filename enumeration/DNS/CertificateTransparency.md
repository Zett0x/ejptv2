# Certificate Transparency Logs

## Introducción
- Los certificados SSL/TLS garantizan la identidad de un sitio web y la comunicación segura.  
- Si una Autoridad Certificadora (CA) emite certificados falsos o maliciosos, pueden usarse para **suplantar sitios** o interceptar datos.  
- Los **CT Logs** son registros públicos e inmutables que guardan cada certificado emitido por las CAs.

## Objetivos de los CT Logs
- **Detección temprana** de certificados fraudulentos o mal emitidos.  
- **Responsabilizar a las CAs**, ya que todo queda registrado públicamente.  
- **Fortalecer la Web PKI**, asegurando transparencia y verificabilidad en el ecosistema de certificados.

## Cómo funcionan
1. **Emisión del certificado** → la CA valida al solicitante y crea un *pre-certificado*.  
2. **Envío al CT log** → el *pre-certificado* se registra en varios logs (redundancia y descentralización).  
3. **SCT (Signed Certificate Timestamp)** → prueba criptográfica que confirma que fue registrado.  
4. **Verificación en el navegador** → el browser comprueba que el certificado tenga SCT válido en los logs públicos.  
5. **Monitoreo y auditoría** → investigadores, dueños de sitios y navegadores vigilan en busca de anomalías.

## Estructura criptográfica (Merkle Tree)
- Los certificados se organizan en un **árbol de Merkle**.  
- Cada hoja = un certificado.  
- Los nodos intermedios = hashes de sus hijos.  
- El **Merkle Root** representa el estado completo del log.  
- Permite verificar cualquier certificado sin descargar todo el log, garantizando detección de manipulación.

## Uso en Web Recon
- Los CT logs sirven para **enumerar subdominios**:
  - A diferencia del brute force, ofrecen un registro histórico y completo.  
  - Revelan subdominios viejos o ya expirados, potencialmente vulnerables.  
- Beneficio: acceso a subdominios ocultos o difíciles de adivinar.

## Herramientas para consultar CT Logs
| Herramienta | Características | Pros | Contras |
|-------------|----------------|------|---------|
| **crt.sh**  | Búsqueda simple por dominio, muestra certificados y SAN entries. | Gratis, sin registro, fácil de usar. | Pocas opciones de filtrado/análisis. |
| **Censys**  | Motor avanzado, filtros por dominio, IP, atributos. | Datos extensos, API disponible. | Requiere registro (free tier). |

## Ejemplo con crt.sh API
Buscar subdominios que contengan "dev" en `facebook.com`:

```bash
curl -s "https://crt.sh/?q=facebook.com&output=json" \
 | jq -r '.[] | select(.name_value | contains("dev")) | .name_value' \
 | sort -u
```

### Explicación:
- `curl`: obtiene resultados en JSON.  
- `jq`: filtra por campos `name_value` que contengan "dev".  
- `sort -u`: ordena y elimina duplicados.  
