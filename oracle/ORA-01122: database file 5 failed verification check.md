Accidentalmente eliminé un datafile llamado example01.dbf lo cual al iniciar la base de datos daba el siguiente error:

```bash
SQL> alter database open;
alter database open
*
ERROR at line 1:
ORA-01122: database file 5 failed verification check
ORA-01110: data file 5: '/u01/app/oracle/oradata/db/example01.dbf'
ORA-01210: data file header is media corrupt
```

Como no tenía respaldo y además era un ambiente de desarrollo pensé en mejor eliminar ese registro. Sin embargo, para eliminar un datafile se necesita que la base de datos esté abierta, pero no se puede abrir porque el datafile es requerido... una parajoda.

Lo anterior se resuelve de la siguiente manera. Primero, es poner el registro del datafile en modo offiline con la opción de `drop`:

```bash
SQL> startup mount
ORACLE instance started.

Total System Global Area 2516582400 bytes
Fixed Size                  2927528 bytes
Variable Size             671089752 bytes
Database Buffers         1828716544 bytes
Redo Buffers               13848576 bytes
Database mounted.


SQL> ALTER DATABASE DATAFILE '/u01/app/oracle/oradata/db/example01.dbf' OFFLINE;
ALTER DATABASE DATAFILE '/u01/app/oracle/oradata/db/example01.dbf' OFFLINE
*
ERROR at line 1:
ORA-01145: offline immediate disallowed unless media recovery enabled


SQL> ALTER DATABASE DATAFILE '/u01/app/oracle/oradata/db/example01.dbf' OFFLINE drop;

Database altered.
```

Finalmente ya se puede lidiar correctamente con la base de datos:

```bash
SQL> alter database open;

Database altered.
```

