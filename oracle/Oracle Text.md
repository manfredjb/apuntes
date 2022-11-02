Ejemplo de creaciones de distintos tipos de índices para la búsqueda de texto

## Índice de texto con múltiples columnas
Ejemplo de cómo crear un índice llamado `CURSOS_BUSCADOR` para la tabla `cursos` creando una columna virtual llamada `CURSOS_MULTI_COLUMN_DATASTORE` que está compuesta por las columnas `capacitacion` y `des_capacitacion`.

```sql
begin
ctx_ddl.create_preference('CURSOS_MULTI_COLUMN_DATASTORE', 'MULTI_COLUMN_DATASTORE');
ctx_ddl.set_attribute('CURSOS_MULTI_COLUMN_DATASTORE', 'COLUMNS', 'CAPACITACION, DES_CAPACITACION');
end;
/

create index CURSOS_BUSCADOR on CURSOS (CAPACITACION) indextype is ctxsys.context
  parameters ('datastore CURSOS_MULTI_COLUMN_DATASTORE');
```



## Sincronizar los índices

Importante saber que la sincronización de los índices por defecto es "manual", es decir, los índices no se van a sincronizar hasta realizar la tarea manualmente:
```sql
exec ctx_ddl.sync_index('CURSOS_BUSCADOR');
```

### Sincronización con commit

También se puede especificar que el índice se sincronice en cada commit:
```sql
create index CURSOS_BUSCADOR on CURSOS (CAPACITACION) indextype is ctxsys.context
  parameters ('datastore CURSOS_MULTI_COLUMN_DATASTORE sync (on commit)');
```
 >  La última opción es mediante jobs
 
## Composición de un índice
Para verificar cómo está compuesto un índice CTX se puede hacer a través de:
 
```sql
select ctx_report.describe_index ('CURSOS_BUSCADOR') from dual;
```
O bien, para ver el script de creación podemos hacer:

```sql
select ctx_report.create_index_script ('CURSOS_BUSCADOR') from dual;
```

## Eliminar una preferencia:
```sql
ctx_ddl.drop_preference('usuarios_datastore');
```

## Ejemplo de búsquedas

Todos los cursos que llamados `libres`: 
```sql
SELECT a.capacitacion, a.des_capacitacion, a.costo_hora
FROM cursos a
WHERE contains (a.capacitacion, 'libres') > 1
```

Todos los cursos que empiecen con `excel`: 
```sql
SELECT a.capacitacion, a.des_capacitacion, a.costo_hora
FROM cursos a
WHERE contains (a.capacitacion, 'excel%') > 1
```

Buscar todos los cursos que tengan la palabra `excel` y `2010`: 
```sql
SELECT a.capacitacion, a.des_capacitacion, a.costo_hora
FROM cursos a
WHERE contains (a.capacitacion, 'excel and 2010%') > 1
```
