## Certificado SSL en EC2
El siguiente proceso muestra cómo instalar un certificado SSL en un servidor EC2 AMI.

## Terminología
Para poder llevar a cabo un proceso sano y claro es importante conocer algunos términos importantes. Cualquier sugerencia para mejores descripciones son bien recibidas. A continuación algunos conceptos sobre la marcha:

* Llave pública y privada

* Llave pública y privada (También conocida como clave privada RSA)
* Certificado CSR (Certificate Signing Request)
* Certificado CA

### Llave pública y privada (También conocida como clave privada RSA)
Para entender el concepto de llave pública y privada en el contexto de seguridad y enciptación de datos, vamos a suponer que hay una persona llamada Ana que quiere enviar a David un mensaje secreto que solo él pueda leer. Entonces:

> Primero, David envía a Ana una caja abierta, pero con cerradura, cerradura que se bloqueará una vez se cierre la caja, y que sólo podrá abrirse con una llave, que sólo David tiene. Ana recibe la caja, escribe el mensaje, lo pone en la caja y la cierra con su cerradura (ahora Ana ya no podrá abrir la caja para acceder de nuevo al mensaje). Finalmente, Ana envía la caja a David y éste la abre con su llave. 


En este ejemplo, la caja con la cerradura es la `clave pública` de David, y la llave de la cerradura es su `clave privada`. ¿Cómo luce una llave privada? Luce más o menos así:

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEA3Tz2mr7SZiAMfQyuvBjM9Oi..Z1BjP5CE/Wm/Rr500P
RK+Lh9x5eJPo5CAZ3/ANBE0sTK0ZsDGMak2m1g7..3VHqIxFTz0Ta1d+NAj
wnLe4nOb7/eEJbDPkk05ShhBrJGBKKxb8n104o/..PdzbFMIyNjJzBM2o5y
5A13wiLitEO7nco2WfyYkQzaxCw0AwzlkVHiIyC..71pSzkv6sv+4IDMbT/
XpCo8L6wTarzrywnQsh+etLD6FtTjYbbrvZ8RQM..Hg2qxraAV++HNBYmNW
s0duEdjUbJK+ZarypXI9TtnS4o1Ckj7POfljiQI..IBAFyidxtqRQyv5KrD
kbJ+q+rsJxQlaipn2M4lGuQJEfIxELFDyd3XpxP..Un/82NZNXlPmRIopXs
2T91jiLZEUKQw+n73j26adTbteuEaPGSrTZxBLR..yssO0wWomUyILqVeti
6AkL0NJAuKcucHGqWVgUIa4g1haE0ilcm6dWUDo..fd+PpzdCJf1s4NdUWK
YV2GJcutGQb+jqT5DTUqAgST7N8M28rwjK6nVMI..BUpP0xpPnuYDyPOw6x
4hBt8DZQYyduzIXBXRBKNiNdv8fum68/5klHxp6..4HRkMUL958UVeljUsT
BFQlO9UCgYEA/VqzXVzlz8K36VSTMPEhB5zBATV..PRiXtYK1YpYV4/jSUj
vvT4hP8uoYNC+BlEMi98LtnxZIh0V4rqHDsScAq..VyeSLH0loKMZgpwFEm
bEIDnEOD0nKrfT/9K9sPYgvB43wsLEtUujaYw3W..Liy0WKmB8CgYEA34xn
1QlOOhHBn9Z8qYjoDYhvcj+a89tD9eMPhesfQFw..rsfGcXIonFmWdVygbe
6Doihc+GIYIq/QP4jgMksE1ADvczJSke92ZfE2i..fitBpQERNJO0BlabfP
ALs5NssKNmLkWS2U2BHCbv4DzDXwiQB37KPOL1c..kBHfF2/htIs20d1UVL
+PK+aXKwguI6bxLGZ3of0UH+mGsSl0mkp7kYZCm..OTQtfeRqP8rDSC7DgA
kHc5ajYqh04AzNFaxjRo+M3IGICUaOdKnXd0Fda..QwfoaX4QlRTgLqb7AN
ZTzM9WbmnYoXrx17kZlT3lsCgYEAm757XI3WJVj..WoLj1+v48WyoxZpcai
uv9bT4Cj+lXRS+gdKHK+SH7J3x2CRHVS+WH/SVC..DxuybvebDoT0TkKiCj
BWQaGzCaJqZa+POHK0klvS+9ln0/6k539p95tfX..X4TCzbVG6+gJiX0ysz
Yfehn5MCgYEAkMiKuWHCsVyCab3RUf6XA9gd3qY..fCTIGtS1tR5PgFIV+G
engiVoWc/hkj8SBHZz1n1xLN7KDf8ySU06MDggB..hJ+gXJKy+gf3mF5Kmj
DtkpjGHQzPF6vOe907y5NQLvVFGXUq/FIJZxB8k..fJdHEm2M4=
-----END RSA PRIVATE KEY-----
```

La llave pública tiene el mismo formato, y puede ser ser extraída de la misma llave privada. Las llaves o claves privadas se pueden crear con un software llamado SSL que se pueden encontrar tanto en Linux como en Windows. En este ejemplo vamos a llevar todo el proceso en el ambiente Linux AMI de AWS.

### Certificado CSR (Certificate Signing Request)
Este certificado contendrá información codificada específica para su compañía y el nombre de dominio, información que se conoce como un nombre completo o DN. El DN para la mayoría de servidores contiene los siguientes campos: País, Estado (o provincia) , Localidad (o ciudad), la Organización de la Unidad Organizacional y nombre común.
> El nombre común o "common name" es donde se define el dominio que se desea proteger, acá por ejemplo, el common name sería sub.ejemplo.com

Así luce un certificado CSR:
```
-----BEGIN CERTIFICATE REQUEST-----
MIIByjCCATMCAQAwgYkxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlh
MRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRMwEQYDVQQKEwpHb29nbGUgSW5jMR8w
HQYDVQQLExZJbmZvcm1hdGlvbiBUZWNobm9sb2d5MRcwFQYDVQQDEw53d3cuZ29v
Z2xlLmNvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEApZtYJCHJ4VpVXHfV
IlstQTlO4qC03hjX+ZkPyvdYd1Q4+qbAeTwXmCUKYHThVRd5aXSqlPzyIBwieMZr
WFlRQddZ1IzXAlVRDWwAo60KecqeAXnnUK+5fXoTI/UgWshre8tJ+x/TMHaQKR/J
cIWPhqaQhsJuzZbvAdGA80BLxdMCAwEAAaAAMA0GCSqGSIb3DQEBBQUAA4GBAIhl
4PvFq+e7ipARgI5ZM+GZx6mpCz44DTo0JkwfRDf+BtrsaC0q68eTf2XhYOsq4fkH
Q0uA0aVog3f5iJxCa3Hp5gxbJQ6zV6kJ0TEsuaaOhEko9sdpCoPOnRBm2i/XRD2D
6iNh8f8z0ShGsFqjDgFHyF3o+lUyj+UC6H1QW7bn
-----END CERTIFICATE REQUEST-----
```

### Certificado CA
Un certificado CA es un certificado digital emitido por una entidad emisora de certificados, por ejemplo, RapidSSL, DigiCert, GoDaddy, etc. Durante el proceso de creación de este certificado se incluye información de la entidad, persona u organización que los remote, como el nombre, correo, dominio donde se emite, teléfono, dirección y más. ¿Cómo luce un certificado CA? Luce algo así:

```
-----BEGIN CERTIFICATE-----
MIIEczCCA1ugAwIBAgIBADANBgkqhkiG9w0BAQQFAD..AkGA1UEBhMCR0Ix
EzARBgNVBAgTClNvbWUtU3RhdGUxFDASBgNVBAoTC0..0EgTHRkMTcwNQYD
VQQLEy5DbGFzcyAxIFB1YmxpYyBQcmltYXJ5IENlcn..XRpb24gQXV0aG9y
aXR5MRQwEgYDVQQDEwtCZXN0IENBIEx0ZDAeFw0wMD..TUwMTZaFw0wMTAy
MDQxOTUwMTZaMIGHMQswCQYDVQQGEwJHQjETMBEGA1..29tZS1TdGF0ZTEU
MBIGA1UEChMLQmVzdCBDQSBMdGQxNzA1BgNVBAsTLk..DEgUHVibGljIFBy
aW1hcnkgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkxFD..AMTC0Jlc3QgQ0Eg
THRkMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCg..Tz2mr7SZiAMfQyu
vBjM9OiJjRazXBZ1BjP5CE/Wm/Rr500PRK+Lh9x5eJ../ANBE0sTK0ZsDGM
ak2m1g7oruI3dY3VHqIxFTz0Ta1d+NAjwnLe4nOb7/..k05ShhBrJGBKKxb
8n104o/5p8HAsZPdzbFMIyNjJzBM2o5y5A13wiLitE..fyYkQzaxCw0Awzl
kVHiIyCuaF4wj571pSzkv6sv+4IDMbT/XpCo8L6wTa..sh+etLD6FtTjYbb
rvZ8RQM1tlKdoMHg2qxraAV++HNBYmNWs0duEdjUbJ..XI9TtnS4o1Ckj7P
OfljiQIDAQABo4HnMIHkMB0GA1UdDgQWBBQ8urMCRL..5AkIp9NJHJw5TCB
tAYDVR0jBIGsMIGpgBQ8urMCRLYYMHUKU5AkIp9NJH..aSBijCBhzELMAkG
A1UEBhMCR0IxEzARBgNVBAgTClNvbWUtU3RhdGUxFD..AoTC0Jlc3QgQ0Eg
THRkMTcwNQYDVQQLEy5DbGFzcyAxIFB1YmxpYyBQcm..ENlcnRpZmljYXRp
b24gQXV0aG9yaXR5MRQwEgYDVQQDEwtCZXN0IENBIE..DAMBgNVHRMEBTAD
AQH/MA0GCSqGSIb3DQEBBAUAA4IBAQC1uYBcsSncwA..DCsQer772C2ucpX
xQUE/C0pWWm6gDkwd5D0DSMDJRqV/weoZ4wC6B73f5..bLhGYHaXJeSD6Kr
XcoOwLdSaGmJYslLKZB3ZIDEp0wYTGhgteb6JFiTtn..sf2xdrYfPCiIB7g
BMAV7Gzdc4VspS6ljrAhbiiawdBiQlQmsBeFz9JkF4..b3l8BoGN+qMa56Y
It8una2gY4l2O//on88r5IWJlm1L0oA8e4fR2yrBHX..adsGeFKkyNrwGi/
7vQMfXdGsRrXNGRGnX+vWDZ3/zWI0joDtCkNnqEpVn..HoX
-----END CERTIFICATE-----
```

Para crear un certificado CA se necesita una llave privada tal como se mencionó anteriormente. Esto es importante porque ya nos va dando un flujo de proceso sobre qué debemos generar primero.

## Prerequsitios
Antes de empezar, verificar que:

* El puerto 443 esté abierto en los Inbound rules del servidor, ya que el protocolo HTTPS trabaja bajo este puerto.
* El servidor apache esté instalado

### 1. Instalar SSL/TLS en el servidor
Es un componente para la gestión de certificados y llaves. Nos permite crear certificados, crear llaves públicas y privadas, desencriptar información usando llaves, etc. Entonces, instalamos:

```
# sudo yum update -y
# sudo yum install -y mod24_ssl
```

Más adelante vamos a trabajar con los siguientes archivos:

* /etc/httpd/conf.d/ssl.conf
* /etc/pki/tls/private/localhost.key
* /etc/pki/tls/certs/localhost.crt

Reiniciamos apache, porque SSL también actúa como un módulode Apache, y como tal debemos configurarle más adelante.

```
sudo service httpd restart
```

### 2. Crear una llave privada
Este llave se crea desde nuestro servidor. Como usuario root ($ sudo su -), ingresamos a las rutas donde se almacenan las llaves privadas en `/etc/pki/tls/private/`. Por defecto, el servidor ya contiene una llave privada para ser usada, se llama `localhost.key` y la pueden ver haciendo `ls -l`. Sin ningún problema pueden usar esta llave para el resto del proceso, sin embargo, para generar una nueva se ejecuta el siguiente comando
```
$ sudo openssl genrsa -out mi-llave-privada.key 2048
```
> El archivo resultante, mi-llave-privada.key, es una llave privada RSA de 2048 bits. 

Ahora, hay que asegurarse de que la nueva clave privada posea los permisos correspondientes

```
$ sudo chown root.root mi-llave-privada.key
$ sudo chmod 600 mi-llave-privada.key
$ ls -al mi-llave-privada.key
```

> -rw------- root root mi-llave-privada.key

### 3. Crear el certificado CSR 
El certificado se crea a partir de una llave creada, entonces, procedemos:

    $ sudo openssl req -new -key mi-llave-privada.key -out csr.pem

> El archivo resultante, `csr.pem`, contiene la clave pública, la firma digital de la clave pública y los metadatos que ha especificado. 

### 2. Obtener un certificado CA
En este ejemplo he adquirido un certificado en RapidSSL para instalarlo en un dominio ya preconfigurado. Para efectos de este ejercicio se usará el subdominio `sub.ejemplo.com`.

Una vez comprado el certificado viene el proceso de generación del certificado, y para ello debemos contar con un certificado firmado por nuestro propio servidor, que el punto 3 lo llamamos `csr.pem`.

Este proceso suele consistir en abrir el archivo CSR en un editor de texto y copiar el contenido, ya que durante el proceso de generación del certificado CA es requerido. En el caso de RapidSSL, luego de la compra del certificado es usuario es redirigido a un formulario para la generación del mismo usando el contenido el archivo CSR.

Una vez completado el formulario, se generará una solicitud de aprobación por parte del proveedor. Aprobada su solicitud, recibirá un nuevo certificado de host firmado por la CA. Es posible que también tenga que descargar un archivo de certificado intermedio que contiene información adicional para poder procesar los certicados CA.

```
$ sudo su -
# cd /etc/pki/tls/private/
```
