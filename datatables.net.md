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
* `serverSide`: La paginación y el filtro de datos corre por parte del servidor
* `columns`: Las columnas asociadas a la tabla html

### Parámetros de envío
Cuando el objeto DataTable hace la solicitud al servidor, éste envía 2 parámetros que sirven para paginar los registros:

* start: Número del primer registro
* length: Cantidad de registros por página

Muchas veces en el servidor remoto, estas variables pueden tener otros nombres, por ejemplo "pagina" o "limitePorPagina". Para no cambiar código del lado del servidor, se puede hacer un cambio en DataTable para que renombre esos parámetros. Para ello se usa la función `data()`. Ejemplo:

```js
ajax: {
	url: 'http://localhost/personas/api.php',
	//dataSrc: 'data',
	data: function (d) {
		d.pagina = d.start
		d.limitePorPagina = d.length
	},
	
```
> Lo que soluciona el bloque anterior es que, `pagina` tendrá el valor de `start` y `limitePorPagina` tendrá el valor de  `length`.

### Parámetros de respuesta
Lo mismo sucede con la respuesta de parte del servidor, DataTable espera por defecto un bloque de la siguiente manera:
```js
{
	"recordsTotal": // cantidad de registros totales en la db
	"recordsFiltered": // cantidad de registros filtrados
	"data": // datos  filtrados o paginados
}
```


En DataTable se hace uso de la función DataFilter para realizar dicho reemplazo en caso de que el servidor devuelve una estructura diferente, por ejemplo:

```js
{
	"totalRegistros": // cantidad de registros totales en la db
	"totalFiltrados": // cantidad de registros filtrados
	"datos": // datos  filtrados o paginados
}
```

Se procede a realizar el reemplazo:

```js
ajax: {
	url: 'http://localhost/personas/api.php',
	dataFilter: function(data){
		const json = JSON.parse(data);
		json.data = json.datos;
		json.recordsFiltered = json.totalFiltrados;
		json.recordsTotal = json.totalRegistros;
		return JSON.stringify(json);
	}
}
```

Finalmente, un ejemplo completo quedaría de la siguiente manera:

```js
let table = new DataTable('#tabla', {
	ajax: {
		url: 'http://localhost/personas/api.php',
		//dataSrc: 'data',
		data: function (d) {
			d.pagina = d.start
			d.limitePorPagina = d.length
		},
		dataFilter: function(data){
			const json = JSON.parse(data);
			json.data = json.datos;
			json.recordsFiltered = json.totalFiltrados;
			json.recordsTotal = json.totalRegistros;
			return JSON.stringify(json);
		}
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
