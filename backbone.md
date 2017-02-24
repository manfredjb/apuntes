Modelo
=======
Estructura de un modelo:

```javascript
var Modelo = Backbone.Model.extend({

    defaults:{
        id: null,
        calificacion: 0,
        nombre: ''
    },
    idAttribute: "id",
    urlRoot: 'localhost/api/modelo'
});
```

Atributos propiamente del modelo:

* **defaults**: Contiene la lista de atributos del objeto como tal
* **idAttribute**: Especifica el nombre del atributo que será usado como identificador dentro de colecciones y restful
* **urlRoot**: Especifica la url del web service que tiene las acciones CRUD

###¿Cómo asignar dinámicamente la urlRoot?


Se puede hacer mediante una función:

```javascript
urlRoot: function(){
  return this.get('url')
}
```

##Vista
Definición de una vista:

```javascript
var Vista = Backbone.View.extend({
    el: '#actividades',

    events:{
        'change .calificacion': 'calificar'
    },

    initialize: function(parametros){
        _.bindAll(this, 'calificar', 'enEliminar', 'render');
        this.model = new GestorActividades(parametros);
        this.model.on('enEliminar', this.enEliminar);
        this.render();
    },
    
    render: function(){
    }
});
```

La vista se compone principalmente por los siguientes elementos:

* **el**: Elemento que encapsula toda la vista (HTML) que contiene los elementos donde se van a desempeñar todas las acciones
* **event**: Lista de eventos asociados a los elementos.
initialize

###Eventos
####Bootstrap
Escuchar el evento cuando un modal es mostrado:
```javascript
    "show.bs.modal #dialogo-terminos": "mostrarDialogoSolicitud",
```

>`#dialogo-terminos` es el valor de `data-target` definidor en el botón.

####Acceso al elemento asociado al evento
Accesar al elemento que desencadenó el evento.
```javascript
buscarPlaza: function(e){
    e.preventDefault();
    var $boton = $(e.target);
}
```

####Eventos manuales
Permite al modelo desencadenar un evento manualmente mientras que la vista escucha este mismo evento.
```javascript
// modelo
modelo.trigger('mostrarNombre', {nombre: "hola"});

// vista
initialize: function(parametros){
    ...
    this.listenTo(this.model, 'mostrarNombre', this.enMostrarNombre);
},
    
    
enMostrarNombre: function (datos) {
    console.log(datos.nombre);
},
```


###template()
Esta función permite manipular contenido dentro de un elemento dom. Es útil cuando necesitamos agregar contenido dinámicamente.
```html
<script type="text/html" id="plantilla-texto-solicitud">
    <p>Hola <%= descripcion %></p>
</script>
```
> Esta plantilla debe estar definida fuera del contexto de la vista.

Ahora sustituimos valores y agregamos el contenido resultado a un elemento de la vista.

```javascript
var plantilla = _.template($('#plantilla-texto-solicitud').html());
$('#dialogo-solicitud-texto').html(plantilla({
    "descripcion": "mi descripción de prueba"
}));
```

##Colecciones

Estructura de una colección:
```javascript
var ColeccionCursos = Backbone.Collection.extend({
    model: Curso,
    idAttribute: 'codigo'
});
```

###Métodos más comunes
Agregar:
```javascript
coleccion.add(curso);
```

Vaciar:
```javascript
coleccion.reset();
```

Buscar un elemento dado un objeto. Usa el idAttribute para encontrarlo:
```javascript
var encontrado = coleccion.get(curso);
```

Buscar un objeto por sus propiedades:
```javascript
var modelo = coleccion.find({
    "nombre": "ABC"
});
```

Buscar todos los objetos que cumplan un criterio y devuelve un `array` con los resultados :
```javascript
var modelos = coleccion.where({
    "color": "azul",
    "tamaño": 3
});
```

>La cantidad de elementos se puede saber usando la propiedad `length`

Verificar si existe un elemento:
```javascript
// true o false
var existencia = coleccion.contains(curso);
```

Verificar si existe un elemento basado en un criterio:
```javascript
var existe = this.get('palabras').find(function(p){
    return p == 'hola';
});
// true
console.log(existe);
```

Eliminar elementos:
```javascript
// elimina un solo elemento
var modelos = coleccion.remove(curso);

// elimina una lista de elemento
coleccion.remove(cursos);
```

Iterar:
```javascript
matriculados.each(function(matricula) {
    console.log("nombre: " + matricula.get('nombre'));
});
```

Tamaño de una colección:
```javascript
var tamano = matriculados.size();
```

Sumar el valor de un atributo de todos los modelos:
```javascript
totalHorasInvertidas: function () {
        return _.reduce(this, function(memo, miModelo){ return memo + miModelo.get('hora'); }, 0);
    },
```
> En este ejemplo se suman las horas de todos los modelos a partir de 0. `memo` es una variable temporal que llevará el control de la suma.


##Problemas frecuentes
Algunos problemas que son frecuentes por dejar de poner atención:

###Los eventos no se despechan en contenido dinámico
Cuando agregamos html dinánicamente a la vista, a veces los nuevos eventos asociados a los dom agregados no son ejecutados, por lo tanto se debe revisar que:

1. Si se están usando templates de tipo `<script type"text/html">` se deben sacar de la vista.
2. El selector del elemento `el` de la vista debe de existir en el html. Cuando no coinciden los eventos no se delegan nunca.
  
  >http://stackoverflow.com/questions/15158489/jquery-backbone-click-events-not-firing

###Otros tutoriales

http://rahulrajatsingh.com/2014/07/backbone-tutorial-part-5-understanding-backbone-js-collections/
http://www.codeproject.com/Articles/797899/BackBone-Tutorial-Part-CRUD-Operations-on-Backbone
