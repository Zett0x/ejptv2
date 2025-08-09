## Directory
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local -t 100 -r

gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://94.237.61.242:30147 -r -t 100


## Files
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local -t 100 -r -x cgi,txt,php

## Subdomains

### vhost
gobuster vhost -u http://titanic.htb -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain -r

### dns


```bash
gobuster dns -d DOMAIN -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```

- **`-d`** → Dominio objetivo.
    
- **`-w`** → Wordlist de subdominios.
    
- Busca **subdominios reales** que tengan entrada DNS pública.
    
- Útil cuando el dominio resuelve en Internet o en un DNS interno.
    

**Ejemplo:**

```bash
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/namelist.txt
```

