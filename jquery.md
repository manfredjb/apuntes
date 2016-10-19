###Estados de los elementos
Verificar el estados de los elementos para según la propiedad `disabled` o `checked`:

```javascript
if ($el.prop('disabled')){
}

if ($el.prop('checked')){
}
```

### data()
Importante recordar que este método trata de representar el valor de `data-*` lo más aproximado al tipo de javascript. Para extraer el valor de `data-*` siempre como un string se debe usar `attr()`

###DOM
Seleccionar elementos por atributos:
```html
<input type="text" data-id="3" data-nombre="abc" class=".alerta">
<input type="text" data-id="3" data-nombre="def" class=".alerta">
```
Ejemplo:
```javascript
var $el = $('input[data-id="3"][data-nombre="abc"]');
var $el = $('.alerta[data-nombre="def"]');
```

