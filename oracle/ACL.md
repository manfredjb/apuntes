`Access Control List` es un componente de las bases de datos de Oracle que permite otorgar o denegar permisos para ciertas funcionalidades. Ya se para enviar un correo, ejecutar un script o una URL o conectarse a otro servidor.

## Uso de SMTP para enviar correos
Posiblemente cuando intentamos enviar un correo, un error como el siguiente fue mostrado:

```
ORA-24247: network access denied by access control list (ACL)
ORA-06512: at "SYS.UTL_TCP", line 17
ORA-06512: at "SYS.UTL_TCP", line 246
ORA-06512: at "SYS.UTL_SMTP", line 127
ORA-06512: at "SYS.UTL_SMTP", line 150
ORA-06512: at "RH.MAIL_FILES", line 135
ORA-06512: at line 3
```

Para ello, debemos verificar si existe el permiso desde ACL:

```sql
SELECT host, lower_port, upper_port, acl FROM dba_network_acls ;
```

Si no hay registros, procedemos a crear una nueva lista:

```sql
BEGIN
  DBMS_NETWORK_ACL_ADMIN.create_acl (
    acl          => 'acl_rh.xml', 
    description  => 'ACL RH',
    principal    => 'RH',
    is_grant     => TRUE, 
    privilege    => 'connect',
    start_date   => SYSTIMESTAMP,
    end_date     => NULL);

  COMMIT;
END;
/
```

Otorgamos los permisos para la conexión a un nuevo servidor de correo:

```sql
BEGIN
  DBMS_NETWORK_ACL_ADMIN.assign_acl (
    acl => 'acl_rh.xml',
    host => '127.0.0.1', 
    lower_port => 8181,
    upper_port => NULL); 
END;
/
```

Volvemos a verificar y listo:

```sql
SELECT host, lower_port, upper_port, acl FROM dba_network_acls ;
```
