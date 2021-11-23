## Apache
Iniciar:
```
httpd -k start
```

## Puertos
Verificar los puertos que están escuchando:
```
netstat -anob
```

Verificar qué se escucha en el puerto 80:
```
netstat -anob | find ":80"
```

## Tareas
Mostrar el nombre de la tarea por PID:
```
tasklist /fi "pid eq 4"
```

Matar una tarea por PID:
```
taskkill /F /pid 4
```

Matar una tarea por nombre:
```
taskkill /IM Toad.exe /F
```
