## Filesystem
Cuando el disco se acerca al límite de almacenamiento podemos realizar algunas tareas de mantenimiento.

1. Buscar carpetas que tengan muchos archivos pesados:

    ```bash
    # du -h --max-depth=2
    
    4.0K    ./databases
    4.0K    ./cpbackup/weekly
    4.0K    ./cpbackup/monthly
    12K     ./cpbackup
    20K     .
    ```
    
2. Mostrar los logs más pesados:

    ```bash
    # ls -lahS /var/log/
    
    -rw------- 1 root root 3.0G Jul 3 03:40 messages-20160703
    -rw------- 1 root root 2.8G Jun 19 03:36 messages-20160619
    -rw------- 1 root root 2.7G Jul 10 03:29 messages-20160710
    -rw------- 1 root root 2.6G Jun 26 03:50 messages-20160626
    ```
    
    > Se pueden comprimir mediante `tar -cvzf messages-20160703.tar messages-20160703`

## Eliminar gran volumen de archivos
Cuando la cantidad de archivos es muy extensa, salta el siguiente mensaje al hacer un `rm`:

```
$ rm -rf *
-bash: /usr/bin/rm: Argument list too long
```

Por lo tanto, se puede usar el siguiente script para eliminar sin problemas:
```
find /path -type f -mtime +1 -exec rm -rf {} \; 
```

* `/path`: directorio donde están los archivos
* `-type f`: considerar solo archivos
* `-mtime +1`: solo archivos de más de un día de antigüedad
