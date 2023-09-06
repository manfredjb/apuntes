Matar todas las sesiones que tienen más de 30 segundos en espera:

```plsql
declare

    cursor csesiones is
    select * from v$session 
    where seconds_in_wait > 30
    and state = 'WAITING'
    and username = 'USUARIO';

BEGIN



  FOR r IN csesiones
  LOOP
      EXECUTE IMMEDIATE 'alter system kill session ''' || r.sid  || ',' || r.serial# || ''' immediate';
  END LOOP;
END;
```
