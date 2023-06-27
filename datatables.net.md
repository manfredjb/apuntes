## Paginación con ajax
Este ejercicio requiere de 3 consideraciones:

1. Configuración base
2. Parámetros de envío
3. Parámetros de respuesta

### Configuración base
Al inicializar el objeto DataTable debe usar los siguientes parámetros:

```js
let table = new DataTable('#tabla', {
            ajax: {
                url: 'http://localhost/personas/api.php',
            },
            processing: true,
            serverSide: true,
            columns: [
                { data: 'name' },
                { data: 'language' },
                { data: 'id' },
                { data: 'bio' },
                { data: 'version' }
            ]
        });
```

Explicación:
* `url`: Endpoint donde se encuentran los datos.
* `processing`: Mostrar un ícono de espera
* `serverSid`: La paginación y el filtro de datos corre por parte del servidor
* `columns`: Las columnas asociadas a la tabla html

### Parámetros de envío
Del lado del servidor, no 
