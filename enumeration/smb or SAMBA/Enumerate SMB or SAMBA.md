
## smbmap

Podemos utilizar esta herramienta para ver los recursos que tenemos acceso y que tipo de acceso tenemos.
command: `smbmap -H <ip> -u admin -p password`

## smbclient

Podemos utilizarla también para listar recursos compartidos dando una null session:
`smbclient -L //ip -N`
o proporcionando usuario y contraseña validos:
`smbclient -L //ip -U username`

Esta herramienta la utilizamos para entrar a un recurso en concreto:

`smbclient //targetip/recuso_compartido -U username `

## enum4linux
Esta herramienta permite sacar una gran cantidad de información de un servicio SMB
`enum4linux -a <ip> -u username -p password`
enum4linux -a -u admin -p password1 demo.ine.local