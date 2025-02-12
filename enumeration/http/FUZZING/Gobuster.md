## Directory
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local -t 100 -r

## Files
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local -t 100 -r -x cgi,txt,php