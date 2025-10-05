## Directory
```bash
wfuzz -c --hc=404 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local/FUZZ -t 100 -L
```

```bash
wfuzz -c --hc=404 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://ftp.soulmate.htb/FUZZ -t 100 -L
```
notas:
--hl=90: Oculta respuestas con length 90 
### VHOST
```bash
wfuzz -H "Host: FUZZ.something.com" --hc 404,403 -H "User-Agent: Chrome" -c -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 100 $target
```

### FILES
```bash
wfuzz -c -t 50 \
  -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt \
  -z list,.txt,.zip,.bak,.conf,.sql \
  --hc 404,403 \
  http://environment.htb/storage/files/FUZZFUZ2Z
```
NOTA: No interesa el 403 porque buscamos ficheros, o mejor dicho, solo interesa 200
## wordpress plugins listing

`wfuzz -c --hc=404 -w /usr/share/nmap/nselib/data/wp-plugins.lst -u http://target2.ine.local/wp-content/plugins/FUZZ -L -t 100`