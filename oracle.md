Secuencias
========
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
insert into libros values (SEQUENCE SECUENCIA_NOTICIA.nextval, 'Tío conejo');
```