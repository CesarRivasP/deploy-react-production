## Despliegue de aplicacion  React  con Nginx

### Primeras instrucciones a realizar:
Desde la terminal:
```
sudo mkdir /var/www/alv-group
sudo gpasswd -a "$USER" www-data
sudo chown -R "$USER":www-data /var/www
find /var/www -type f -exec chmod 0660 {} \;
sudo find /var/www -type d -exec chmod 2770 {} \;
```

### Debes ingresar a la carpeta de configuración de nginx  de la siguiente manera:
```
cd ../../
cd /etc/nginx/sites-available
```
### Una vez te encuentras en la carpeta **sites-available**, debes crear un archivo de la siguiente manera

`touch alv-group-production`

### Para agregar el contenido a ese archivo lo puedes realizar mediante **nano** de la siguiente manera:
`sudo nano alv-group-production`

### Una vez ingresas al editor de nano, agregar el siguiente contenido:

```

server {
  listen 80;
  server_name DIRECCION_IP_DEL_SERVIDOR;
  root /var/www/alv-group;
  index index.html;
  
  access_log /var/log/nginx/alv-group-production.access.log;
  error_log /var/log/nginx/alv-group-production.error.log;
  location / {
    try_files $uri /index.html =404;
  }
}

```

**Nota**: donde dice DIRECCION_IP_DEL_SERVIDOR debes ingresar la **dirección ip publica** que tenga el servidor o entorno donde vas a desplegar la pagina web.


### Luego debes hacer un enlace simbólico con la siguiente instrucción
````
sudo ln -s /etc/nginx/sites-available/alv-group-production /etc/nginx/sites-enabled/alv-group-production
````

### Una vez realizados todos los pasos mencionados anteriormente, regresa al directorio principal con:
`cd`


### Ahora debes ingresar a la carpeta donde va a estar alojado el proyecto
`cd /var/www/`

#### Una vez te encuentres en la carpeta 'www', debes clonar el repositorio
`git clone https://gitlab.com/cesarrivas10/alv-group.git`

**Nota**:  al ingresar dicha instruccion te va a pedir ciertas credenciales para poder acceder al repositorio, las cuales te pasaremos por privado.
#### Ingresas al repositorio
`cd alv-group/`

#### Instalas los paquetes necesarios para el proyecto
`npm install`

#### Luego creas la versión de producción se la siguiente manera:
`npm run build`

#### Para configurar el despliegue, ejecuta: 
`npm run deploy`

### Reinicia el servicio de nginx
`sudo service nginx restart `

Nota:
* Revisa la dirección IP del servidor, para confirmar que el despliegue se realizo de la manera correcta
* Si no se puede visualizar la pagina, revisa el status del servidor web con:
```sudo service nginx status ```
