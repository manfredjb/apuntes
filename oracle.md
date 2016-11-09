## Bloques
Bloques de iteración y condicionales.

###for
Ejemplo de un `for` que iterar de 1 a 20:

```sql
FOR NUMERO IN 1..20 LOOP
   RESULTADO := NUMERO * 3
END LOOP;
```
También se puede iterar de forma inversa:

```sql
FOR NUMERO IN REVERSE 1..20 LOOP
   RESULTADO := NUMERO * 3
END LOOP;
```

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

###Valor actual de la secuencia
Cambiar el valor actual de una secuencia:
```sql
alter sequence SECUENCIA_NOTICIA increment by 500;
```

Llamamos al siguiente valor:
```sql
select SECUENCIA_NOTICIA.nextval from dual;
```

Dejamos al secuencia a su normalidad:
```sql
alter sequence SECUENCIA_NOTICIA increment by 1;
```


##Exportar e importar
Ejemplo de exportación de datos de 11g a 10g:
```sql
expdp usuario/contrasena@db VERSION=10.2 DIRECTORY=DATA_PUMP_DIR FULL=Y LOGFILE=DEMO.log DUMPFILE=DATOS.dmp
```

Para visualizar los directorios disponibles, se pueden ver con:
```sql
SELECT * FROM all_directories;
```

Ejemplo de importación del archivo generado:
```sql
impdp usuario/contrasena@db DIRECTORY=DATA_PUMP_DIR FULL=Y LOGFILE=DEMO.log DUMPFILE=DATOS.dmp
```

##Cursores
Declaración de un cursor:
```sql
CURSOR C_EMPLEADO IS
SELECT NOMBRE, APELLIDO1, CORREO
FROM EMPLEADOS 
WHERE CEDULA = X_CEDULA;
EMPLEADO C_EMPLEADO%ROWTYPE;
```

Abrir y cerrar un cursor:
```sql
OPEN  C_EMPLEADO;
FETCH C_EMPLEADO INTO EMPLEADO;
CLOSE C_EMPLEADO;
```

Iterar un cursor:
```sql
OPEN  C_PUESTOS;
FETCH C_PUESTOS INTO PUESTO;
WHILE C_PUESTOS%FOUND LOOP

   DBMS_OUTPUT.put_line (PUESTO.NOMBRE);

FETCH C_PUESTOS INTO PUESTO;
END LOOP;
CLOSE C_PUESTOS;
```

##Otros comandos

SELECT * FROM v$version;

##Índices
Ejemplo de creaciones de distintos tipos de índices.

###Índice de texto con múltiples columnas
Ejemplo de cómo crear un índice llamado `CURSOS_BUSCADOR` para la tabla `cursos` creando una columna virtual llamada `CURSOS_MULTI_COLUMN_DATASTORE` que está compuesta por las columnas `capacitacion` y `des_capacitacion`.

```sql
begin
ctx_ddl.create_preference('CURSOS_MULTI_COLUMN_DATASTORE', 'MULTI_COLUMN_DATASTORE');
ctx_ddl.set_attribute('CURSOS_MULTI_COLUMN_DATASTORE', 'COLUMNS', 'CAPACITACION, DES_CAPACITACION');
end;
/

create index CURSOS_BUSCADOR on CURSOS (as) indextype is ctxsys.context
  parameters ('datastore CURSOS_MULTI_COLUMN_DATASTORE');
```

Todos los cursos que llamados `libres`: 
```sql
SELECT a.capacitacion, a.des_capacitacion, a.costo_hora
FROM cursos a
WHERE contains (a.capacitacion, 'libres') > 1 and rownum <= ?
```

Todos los cursos que empiecen con `excel`: 
```sql
SELECT a.capacitacion, a.des_capacitacion, a.costo_hora
FROM cursos a
WHERE contains (a.capacitacion, 'excel%') > 1 and rownum <= ?
```

Buscar todos los cursos que tengan la palabra `excel` y `2010`: 
```sql
SELECT a.capacitacion, a.des_capacitacion, a.costo_hora
FROM cursos a
WHERE contains (a.capacitacion, 'excel and 2010%') > 1 and rownum <= ?
```
##Constraint

Eliminar un constraint como una llave primaria:
```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```

##Enlaces primordiales

* [Configuración y administración del pool de conexiones](http://www.toadworld.com/platforms/oracle/w/wiki/1633.database-resident-connection-pooling-drcp)
