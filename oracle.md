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

###Enlaces primordiales

* [Configuración y administración del pool de conexiones](http://www.toadworld.com/platforms/oracle/w/wiki/1633.database-resident-connection-pooling-drcp)
