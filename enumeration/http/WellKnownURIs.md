# .Well-Known URIs

## Introducción
- Definidas en **RFC 8615**.  
- Ubicación estándar en el dominio raíz: `/.well-known/`.  
- Contiene **metadatos críticos** y configuraciones sobre servicios, protocolos y seguridad de un sitio.  
- Objetivo → facilitar descubrimiento automático de configuraciones por navegadores, apps y herramientas de seguridad.  

Ejemplo:  
```text
https://example.com/.well-known/security.txt
```

---

## Ejemplos comunes (IANA Registry)
| URI Suffix | Descripción | Estado | Referencia |
|------------|-------------|--------|------------|
| `security.txt` | Contacto para reportar vulnerabilidades | Permanente | RFC 9116 |
| `change-password` | Redirección estándar para cambiar contraseña | Provisional | https://w3c.github.io/webappsec-change-password-url/#the-change-password-well-known-uri |
| `openid-configuration` | Configuración de OpenID Connect sobre OAuth 2.0 | Permanente | http://openid.net/specs/openid-connect-discovery-1_0.html |
| `assetlinks.json` | Verificación de propiedad de activos digitales (apps ↔ dominio) | Permanente | https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md |
| `mta-sts.txt` | Política de **SMTP MTA-STS** para mejorar seguridad de correo | Permanente | RFC 8461 |

---

## Uso en Web Recon
- Permite descubrir **endpoints** y configuraciones expuestas.  
- Ejemplo: `/.well-known/openid-configuration` (OpenID Connect Discovery).  
  Devuelve JSON con:  
  ```json
  {
    "issuer": "https://example.com",
    "authorization_endpoint": "https://example.com/oauth2/authorize",
    "token_endpoint": "https://example.com/oauth2/token",
    "userinfo_endpoint": "https://example.com/oauth2/userinfo",
    "jwks_uri": "https://example.com/oauth2/jwks",
    "response_types_supported": ["code", "token", "id_token"],
    "subject_types_supported": ["public"],
    "id_token_signing_alg_values_supported": ["RS256"],
    "scopes_supported": ["openid", "profile", "email"]
  }
  ```

### Información que revela
- 🔑 **Authorization Endpoint** → URL para solicitudes de login.  
- 🔑 **Token Endpoint** → dónde se emiten tokens.  
- 📋 **Userinfo Endpoint** → acceso a información de usuario.  
- 🔒 **JWKS URI** → claves criptográficas (JSON Web Key Set).  
- 🎯 **Scopes y Response Types soportados** → funcionalidad y limitaciones.  
- 🛡️ **Algoritmos de firma soportados** → medidas de seguridad (ej. RS256).  

---

## Clave en Reconocimiento
- `/.well-known/` puede revelar información sensible si está mal configurado.  
- Proporciona una forma **estructurada y estandarizada** de descubrir endpoints de seguridad y autenticación.  
- Explorar la **IANA registry** y probar diferentes URIs es útil en pentesting.  
