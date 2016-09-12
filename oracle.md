##Funciones
Crear una función:

```sql
CREATE OR REPLACE FUNCTION 
hello_message 
   (place_in IN VARCHAR2) 
    RETURN VARCHAR2
IS
BEGIN
   RETURN 'Hello ' || place_in;
END hello_message;
```
##Procedimientos
Ejemplos de procedimientos anónimos:

```sql
SET SERVEROUTPUT ON
BEGIN
  DBMS_OUTPUT.put_line ('Hello World!');
END;
```

Usando variable:

```sql
DECLARE
  l_message  
  VARCHAR2 (100) := 'Hello World!';
BEGIN
  DBMS_OUTPUT.put_line (l_message);
END;
```

> Se ejecutan en la consola con un **/** al final

##Secuencias
Crear una secuencia que el valor inicial sea 1, se incremente en 1.

```sql
CREATE SEQUENCE SECUENCIA_NOTICIA
  MINVALUE 1
  START WITH 1
  INCREMENT BY 1
  CACHE 20;
```

Asignar la secuencia el valor de una secuencia a un tabla:
```sql
insert into libros values (SECUENCIA_NOTICIA.nextval, 'Tío conejo');
```

##Exportar
Ejemplo de exportación de datos de 11g a 10g:
```sql
expdp usuario/contrasena@service VERSION=10.2 DIRECTORY=DATA_PUMP_DIR FULL=Y LOGFILE=DEMO.log
```

Para visualizar los directorios disponibles, se pueden ver con:
```sql
SELECT * FROM all_directories;
```

##Otros comandos

SELECT * FROM v$version;

##Enlaces primordiales

* [Configuración y administración del pool de conexiones](http://www.toadworld.com/platforms/oracle/w/wiki/1633.database-resident-connection-pooling-drcp)
