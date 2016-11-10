##Componente autocomplete
Algunos de ejemplos de uso de este componente.

###Enviar parámetros extras
Enviando parámetros adicionales a parte de los que se envía por defecto.
```javascript
$( "#input-buscar" ).autocomplete({
    source: function(request, response) {
        $.getJSON(
            '/buscar-cursos',
            {
                term: request.term,
                limite: 14,
                jefatura: 'Oficina'
            },
            response
        );
    },
    minLength: 4,
}).autocomplete( "instance" )._renderItem = vista.renderizarItem;
```
