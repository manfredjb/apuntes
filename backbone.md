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

¿Cómo asignar dinámicamente la urlRoot?
---

Se puede hacer mediante una función:

```javascript
urlRoot: function(){
  return this.get('url')
}
```

Vista
---

Definición de una vista:

```javascript
var Vista = Backbone.View.extend({
    el: '#actividades',

    events:{
        'change .calificacion': 'calificar'
    },

    initialize: function(parametros){
        _.bindAll(this, 'calificar', 'enEliminar', 'enErrorActualizar', 'enRespuestaDeActualizar', 'render');
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



http://rahulrajatsingh.com/2014/07/backbone-tutorial-part-5-understanding-backbone-js-collections/
http://www.codeproject.com/Articles/797899/BackBone-Tutorial-Part-CRUD-Operations-on-Backbone
