## Apache 2.4
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

### MaxClients
Esta configuración establece la cantidad de conexiones concurrentes que el servidor puede procesar. Para calcular este la cantidad se puede usar la siguiente fórmula:

* Memoria requerida por solicitud: 24MB (estimado)
* Memoria física disponible 8GB
* Memoria requerida por otros procesos externos (sistema operativo, base de datos, etc): 4GB

```
MaxClients = ((8-4) * 1024) / 32
           = 128
```
Para estar es más seguro, podemos redondear a `100` el valor.


### Autenticación con Active Directory

Para ello se hace uso del módulo [mod_authn_ntlm](https://github.com/TQsoft-GmbH/mod_authn_ntlm) para Apache 2.4

#### Instalación
La instalación del módulo depende de los siguientes pasos:

1. Descargar el CMAKE versión Windows X64: https://github.com/Kitware/CMake/releases/download/v3.18.1/cmake-3.18.1-win64-x64.msi
2. Instalar con la opción de "Agregar CMAKE al PATH de ambiente"
3. Descargar el proyecto mod_authn_ntlm
4. Dentro del proyecto hacer:

    ```
    > cmake -B ./build-x64 -S ./ -G "Visual Studio 15 2017" -A x64 -T host=x64 -DAPACHE_ROOT="D:\web\Apache24"
    > cmake --build ./build-x64 --config Release
    > cmake --build ./build-x64 --config Debug
    ```
5. Al terminar, se creará el archivo **mod_authn_ntlm.so** en la carpeta **mod_authn_ntlm/build-x64/Release/**
6. Copiar el contenido de la carpeta release al directorio modules de Apache

#### Configuración
En el archivo httpd.conf habilitar los siguientes módulos:

```ApacheConf
LoadModule ldap_module modules/mod_ldap.so
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule rewrite_module modules/mod_rewrite.so
LoadModule headers_module modules/mod_headers.so
LoadModule auth_ntlm_module modules/mod_authn_ntlm.so
```

#### Ejemplo 
El siguiente código es un ejemplo de un virtual host con proxy con autenticación de AD.

```ApacheConf
<VirtualHost *:9292>
    DocumentRoot "D:\web\apps\prueba-ad"
    ErrorLog "logs/9292-error.log"
    CustomLog "logs/9292-access.log" common
    #<Directory "D:\web\apps\prueba-ad">
	#	Options Indexes FollowSymLinks
    #        AllowOverride All
    #        Require all granted
    #</Directory>
	DirectoryIndex index.php
	ProxyRequests Off
    ProxyPreserveHost On
    ProxyErrorOverride Off
    ProxyPass / http://54.156.66.16:9090/
    ProxyPassReverse / http://54.156.66.16:9090/
	#Header set MyHeader "Hello Joe. It took %D microseconds for Apache to serve this request."
	RequestHeader unset X_ISRW_PROXY_AUTH_USER
	<Location />
		 AuthType SSPI
		 AuthName "Private location"
		 NTLMAuth On
		 NTLMAuthoritative On
		 <RequireAll>
			 <RequireAny>
				 Require valid-user
			 </RequireAny>
			 <RequireNone>
				 Require user "ANONYMOUS LOGON"
			 </RequireNone>
		 </RequireAll>
		 #Header set X_ISRW_PROXY_AUTH_USER expr=%{REMOTE_USER}
		 RequestHeader set X_ISRW_PROXY_AUTH_USER expr=%{REMOTE_USER}
	</Location>
</VirtualHost>
```

#### PHP
Desde PHP, a nivel de proxi se puede conocer el usuaria vía `$_SERVER`:

```php
<?php var_dump($_SERVER); ?>
```
