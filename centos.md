##File system
Cuando el disco se acerca al límite de almacenamiento podemos realizar algunas tareas de mantenimiento.

1. Buscar carpetas que tengan muchos archivos pesados:

    ```terminal
    # du -h --max-depth=2
    
    4.0K    ./databases
    4.0K    ./cpbackup/weekly
    4.0K    ./cpbackup/monthly
    12K     ./cpbackup
    20K     .
    ```
    
2. Mostrar los logs más pesados:

    ls -lahS /var/log/
