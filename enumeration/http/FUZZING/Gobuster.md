## Directory
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local -t 100 -r

gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://94.237.61.242:30147 -r -t 100


## Files
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local -t 100 -r -x cgi,txt,php

## Subdomains

### vhost
gobuster vhost -u http://titanic.htb -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain -r

NOTA: Usad varios diccionarios, usando el arriba descrito no encontré un subdominio y usando
```bash
gobuster vhost -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://planning.htb/ -t 100 -r --append-domain
```
si encontré grafana.planing.htb

There are a couple of other arguments that are worth knowing:

- Consider using the `-t` flag to increase the number of threads for faster scanning.
- The `-k` flag can ignore SSL/TLS certificate errors.
- You can use the `-o` flag to save the output to a file for later analysis.
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

