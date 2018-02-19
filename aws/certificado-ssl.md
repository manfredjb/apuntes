## Certificado SSL en EC2
El siguiente proceso muestra cómo instalar un certificado SSL en un servidor EC2 AMI.

## Prerequsitios
Antes de empezar, verificar que:

* El puerto 443 esté abierto en los Inbound rules del servidor, ya que el protocolo HTTPS trabaja bajo este puerto.
* El servidor apache esté instalado

### 1. Instalar SSL/TLS en el servidor
Es un componente para la gestión de certificados y llaves.

```
# sudo yum update -y
# sudo yum install -y mod24_ssl
```

Más adelante vamos a trabajar con los siguientes archivos:

* /etc/httpd/conf.d/ssl.conf
* /etc/pki/tls/private/localhost.key
* /etc/pki/tls/certs/localhost.crt

Reiniciamos apache

```
sudo service httpd restart
```

### 2. Obtener un certificado CA
En este ejemplo he adquirido un certificado en RapidSSL para instalarlo en un dominio ya preconfigurado. Para efectos de este ejercicio se usará el subdominio `sub.ejemplo.com`.

Una vez comprado el certificado viene el proceso de generación del certificado, y para ello debemos contar con un certificado firmado por nuestro propio servidor.

1. Ir a la ruta `/etc/pki/tls/private/`
2. Si se quiere generar una clave RSA propia puede 

### 2. Crear una llave privada
Este llave se crea desde nuestro servidor. Como usuario root ($ sudo su -), ingresamos a las rutas donde se almacenan las llaves privadas en `/etc/pki/tls/private/`:

```
$ sudo su -
# cd /etc/pki/tls/private/
```

Si hacemos `ls -l` vemos que ya existe un archivo llamado `localhost.key`. Si se sienten cómodo puden usar esta llave privada para generar un certificado:
