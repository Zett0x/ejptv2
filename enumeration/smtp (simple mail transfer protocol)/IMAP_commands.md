# 📬 IMAP – Chuleta de Comandos

## 🔹 Conexión
```bash
telnet servidor.com 143
openssl s_client -connect servidor.com:993 -crlf -quiet   # SSL
```

## 🔹 Autenticación
```imap
A1 LOGIN usuario@dominio.com password   # login sencillo
```

## 🔹 Carpetas
```imap
A2 LIST "" "*"         # listar todas las carpetas
A3 SELECT "INBOX"      # abrir la carpeta INBOX
```

## 🔹 Consultar mensajes
```imap
A4 STATUS "INBOX" (MESSAGES UNSEEN)   # nº de mensajes y no leídos
```

## 🔹 Recuperar mensajes
```imap
A5 FETCH 1 RFC822                         # correo 1 completo
A6 FETCH 1 BODY[HEADER]                   # solo cabeceras
A7 FETCH 1 BODY[TEXT]                     # solo cuerpo
A8 FETCH 1 BODY[HEADER.FIELDS (Subject)]  # solo Subject
```

## 🔹 Buscar mensajes
```imap
A9 SEARCH ALL
A10 SEARCH UNSEEN
A11 SEARCH FROM "juan@dominio.com"
```

## 🔹 Marcar y borrar
```imap
B1 STORE 1 +FLAGS (\Seen)      # marcar como leído
B2 STORE 1 +FLAGS (\Deleted)   # marcar para borrar
B3 EXPUNGE                      # eliminar definitivamente
```

## 🔹 Salir
```imap
ZZ LOGOUT
```
