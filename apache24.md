##Apache 2.4
Configuraciones

### Creación de un virtual host
Una buena práctica es mantener los virtual hosts en un archivo de configuración aparte. Primeramente desde el archivo de configuración `httpd.conf` hacemos la referencia:

```conf
Include conf/extra/httpd-vhosts.conf
```
> Lo ideal es hacer el include luego de las directivas `<directory>`

Creamos el virtual host:
```conf
<VirtualHost *:80>
    ServerAdmin admin@ejemplo.com
    DocumentRoot "c:/Apache24/desarrollo/mi-app/public"
    ServerName mi-app
    ErrorLog "logs/mi-app-error.log"
    CustomLog "logs/mi-app-access.log" common
    <Directory "c:/Apache24/desarrollo/mi-app/public">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Ahora accesamos a la nueva URL de la aplicación:
>http://localhost/desarrollo/mi-app/public/index.php
