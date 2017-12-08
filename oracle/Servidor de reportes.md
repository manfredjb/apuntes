## Exception 50125
Este error se encuentra en el archivo `%ORACLE_HOME%\reports\logs\<servidor>\rwserver.trc`, y propiamente el error se muestra así:

```
[2011/6/5 1:49:28:378] Debug 50103 (RWEngine:init): args[8]=server_ior=
[2011/6/5 1:49:28:409] Info 50128 (RWEngine:init): orb init succeeded
[2011/6/5 1:49:28:409] Exception 50125 (org.omg.CORBA.DATA_CONVERSION: vmcid: 
SUN minor code: 202 completed: No
at com.sun.corba.se.internal.corba.ORB.string_to_object(ORB.java:2038)
```

Lo ideal es que el valor de `server_ior` se vea así:

```
[2011/6/5 11:58:35:969] Debug 50103 (RWEngine:init): args[8]=server_ior=C:\TEMP\rep2315735
[2011/6/5 11:58:35:985] Info 50128 (RWEngine:init): orb init succeeded
```

### Causa
* La variable de ambiente de OAS `REPORTS_TMP` contiene el directorio de archivos temporales. Si el directorio es incorrecto (que no exista, que no tenga permisos, etc) salta este error.

### Solución
1. Asignar el valor de `REPORTS_TMP` desde el regedit dentro del HOME del OAS:
        
        REPORTS_TMP=C:\tmp
