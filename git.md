## Branch
Comandos asociados a `Branch`.

### Mostrar
Mostrar los branch remotos:
```
git branch -r
```

### Eliminar un branch remoto
```
git fetch -p origin
git branch -r -d "manfred/reclutamiento/desarrollo/manfred/formularios"
```

Luego eliminamos los branch locales que tengan referencia a branch remotos que ya no existan:
```
git remote prune origin
```

## CMD
Copiar los archivos modificados en el commit actual a una carpeta.

```cmd
for /f "usebackq tokens=*" %A in (`git diff --name-only`) do echo FA|xcopy "%~fA" "C:\tmp\cambios\%A"
```
* `echo FA` responde al comando xcopy que sobreescriba todo
* `usebackq` permite usar el ouput del comando git y usarlo como valor de entrada para la cláusula `do`
* `%~fA` convierte la salida del comando de git en rutas apropiadas para la copia
* `C:\tmp\cambios\` es donde se copiaran los archivos
