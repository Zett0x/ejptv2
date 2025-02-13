# TCP

`nmap -sS --open -p- -T4 -Pn -n -sVC -v demo.ine.local -oN scan`

# UDP
`nmap -sU -p- -sV --open <ip>`

![[Pasted image 20250212160711.png]]

## SCANNING SMB SERVICE OF A LIST OF IPS
`nmap -sS -p445 -T5 -sVC -Pn -n -v -iL  hosts.txt -oN smb_scan_all`