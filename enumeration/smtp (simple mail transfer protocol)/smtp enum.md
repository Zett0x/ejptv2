## with MetaSploit

### Enum Users

Con el módulo `auxiliary/scanner/smtp/smtp_enum` podemos enumerar usuarios. Es necesario pasarle un diccionario de usuarios . Una vez obtenido un listado de usuarios válidos podemos hacer fuerza fuerza con hydra:
`hydra -l <user> -P <pass_dict> <targetip> ftp` o ssh u otro servicio que consideremos.

