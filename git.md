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
