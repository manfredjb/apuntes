##Apache 2.4
Configuraciones

### Creación de un virtual host
Una buena práctica es mantener los virtual hosts en un archivo de configuración aparte. Primeramente desde el archivo de configuración `httpd.conf` hacemos la referencia:

```ApacheConf
Include conf/extra/httpd-vhosts.conf
```
> Lo ideal es hacer el include luego de las directivas `<directory>`

Creamos el virtual host:
```ApacheConf
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

### Virtual host con nombre de dominio en localhost
Ejemplo de un  virtual host que responde a la url `mi-app.desarrollo`:
```ApacheConf
<VirtualHost mi-app.desarrollo:80>
    ServerAdmin admin@ejemplo.com
    DocumentRoot "c:/Apache24/desarrollo/mi-app/public"
    ServerName mi-app.desarrollo
    ServerAlias www.mi-app.desarrollo
    ErrorLog "logs/mi-app-error.log"
    CustomLog "logs/mi-app-access.log" common
    <Directory "c:/Apache24/desarrollo/mi-app/public">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
* En **Windows** es necesario configurar el archivo `C:\Windows\System32\drivers\etc\hosts` y agregar la entrada para ese dominio:

        127.0.0.1 localhost
        127.0.0.1 mi-app.desarrollo
        
Finalmente accesamos a la aplicación a través de `http://mi-app.desarrollo`.
