## Bloques
Bloques de iteración y condicionales.

### for
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

## Funciones
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
## Procedimientos
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

## Secuencias
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

### Valor actual de la secuencia
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


## Exportar e importar
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

## Cursores
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

## Otros comandos

SELECT * FROM v$version;


## Constraint

Eliminar un constraint como una llave primaria:
```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```

## Fechas (date)
Manipular fechas:
```sql
-- restar 5 minutos a la fecha actual
select sysdate AHORA, sysdate-(60 * 5)/(24*60*60) RESTA_5_MINUTOS from dual;

-- sumar 30 segundos a la fecha actual
select sysdate AHORA, sysdate+(30)/(24*60*60) AGREGA_30_SEGUNDOS from dual;

-- sumar 2 dias a la fecha actual
select sysdate + 2  from dual;
```

### Consultas
Último día del mes:
```sql
SELECT LAST_DAY(SYSDATE) FROM DUAL;
```

Primer día del mes: (Nos movemos al último día del mes, le sumamos 1 día para ir al primer día del mes siguiente, y luego se resta un mes)
```sql
SELECT ADD_MONTHS( (LAST_DAY(SYSDATE) + 1), -1 ) FROM DUAL;
```

Nombre del día de la semana:
```sql
-- FRIDAY
select to_char(SYSDATE, 'DAY') FROM DUAL;

-- FRI
select to_char(SYSDATE, 'DY') FROM DUAL;
```

> El idioma del nombre del día está sujeto al valor de la configuración `NLS_LANG`

Número de semana del mes:
```
-- 2
SELECT TO_CHAR(trunc(TO_DATE('04/07/2017', 'DD/MM/YYYY')), 'W') FROM DUAL;
```

## Configuraciones NLS
Se configuraciones asociadas al idioma, moneda, territorio, formatos de día, fecha, etc. 

### NLS_TERRITORY
* Cambia el día inicial de la semana de `Sunday` a `Monday`

```SQL
-- Sunday
ALTER SESSION SET NLS_TERRITORY = 'AMERICA';
SELECT to_char(trunc(sysdate, 'D'),'Day') as start_day from dual;

-- Monday
ALTER SESSION SET NLS_TERRITORY = 'Mexico';
SELECT to_char(trunc(sysdate, 'D'),'Day') as start_day from dual;A
```

### NLS_LANG
* Cambia el idioma de los mensajes de error e informacimativos:

```SQL
ALTER SESSION SET NLS_LANG = 'ENGLISH';
-- ORA-00904: "@": invalid identifier

ALTER SESSION SET NLS_LANG = 'SPANISH';
-- ORA-00904: "@": identificador no válido
```

## Idioma de la instancia
Por defecto las instalaciones de oracle se muestra en el idioma del sistema operativo. Suponiendo que 11g está instalado en español, se procede a cambiarlo en inglés de la siguiente manera:

1. Primero se cambia el valor de la llave `NLS_LANG` a `american_america.WE8PC850` en el path del home:

      HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\$HOME

2. Se cambia la locación a Estados Unidos en Start -> Control Panel -> Clock, Language, and Region -> Change location -> Location.

3. Reiniciar el listener:

      > C:\11gDB\dbhome_1\BIN\lsnrctl stop
      > C:\11gDB\dbhome_1\BIN\lsnrctl start

Para instalaciones más antiguas como 10g se puede encontrar la llave en:

    HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\Wow6432Node\$HOME
    
Otros idiomas:

* Inglés: american_america.WE8PC850
* Español: LATIN AMERICAN SPANISH_COSTA RICA.WE8MSWIN1252


## Errores
Lista de soluciones para los errores más comunes.

### FRM-92095: Oracle Jnitiator version too low 
Se debe crear una variable de ambiente con la siguiente información:
* Nombre: **JAVA_TOOL_OPTIONS**
* Valor: **-Djava.vendor="Sun Microsystems Inc."**

### ORA-01033: ORACLE initialization or shutdown in progress.
Algunas de las razones principales de error son:

* Filesystem lleno

#### Filesystem lleno
Si este fuera el caso, entonces abrimos consola y ejecutamos:

    > sqlplus /nolog
    SQL> connect / as sysdba

    Connected.

    SQL> shutdown abort

    ORACLE Instance shut down.

    SQL> startup nomount

    ORACLE Instance started

    SQL> alter database mount;

    SQL> alter database open;
    
>  ORA-19809: limit exceeded for recovery files
    
Verificamos cuánto espacio tenemos reservado para los recovery files y cuánto espacio hemos utilizado

    select SPACE_USED/1024/1024/1024 SPACE_USED, SPACE_LIMIT/1024/1024/1024 SPACE_LIMIT from  v$recovery_file_dest;

    SPACE_USED SPACE_LIMIT
    ---------- -----------
    12.4      12.5
    
Eliminamos logs antiguos con RMAN. Abrimos un `cmd` y ejecutamos:

      > RMAN
      RMAN> connect target RH/RH@DB 
      RMAN> DELETE ARCHIVELOG UNTIL TIME 'SYSDATE-30';

Y volvemos a iniciar la base de datos.

## Enlaces primordiales

* [Configuración y administración del pool de conexiones](http://www.toadworld.com/platforms/oracle/w/wiki/1633.database-resident-connection-pooling-drcp)
