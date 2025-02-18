## Directory
`wfuzz -c --hc=404 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -u http://target1.ine.local/FUZZ -t 100 -L`

## wordpress plugins listing

`wfuzz -c --hc=404 -w /usr/share/nmap/nselib/data/wp-plugins.lst -u http://target2.ine.local/wp-content/plugins/FUZZ -L -t 100`