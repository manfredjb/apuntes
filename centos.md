##File system
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
