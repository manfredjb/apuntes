[[_TOC_|levels = 3]]

##La clase Request
Clase de validación para los request enviados.

###Acceder los binds declarados en el Route
Carga el valor como un objeto si así el bind fue declarado de esa manera.
```php
$product = $request->route('product');
```

###Redirigir a otra página con un mensaje de error
Por defecto si la validación falla, el request regresa a la página de donde provino. Podemos hacer que redirija el error/mensajes a otra página:
```php
public function response(array $errors)
{
       return $this->redirector->route('buscador')
       ->withInput($this->except($this->dontFlash))
       ->withErrors($errors);
}
```

###Parsear los mensajes de error a un array de mensajes
Convierte una lista de errores a un objeto `MessageBag` que contiene los mensajes correspondientes
```php
use Illuminate\Support\MessageBag;
$errorBag = new MessageBag($errors);
```

###Emular la función sometimes()
El servicio `Validator` contiene una función llamada `sometimes()` que no está disponible en la clase Request, pero podemos emularla:
```php
protected function getValidatorInstance(){
    $validator = parent::getValidatorInstance();

    $validator->sometimes('dob', 'valid_date', function($input)
    {
        return apply_regex($input->dob) === true;
    });

    return $validator;
}
```

###Sobreescribir mensajes de error
La clase Request permite sobreescribir mensajes de error para un request dado:
```php
public function messages()
{
   return [
       'name.required' => 'Necesita un nombre para continuar!',
       'email.required' => 'También le faltó el correo xD',
   ];
}
```

###Manipular valores antes de aplicar las reglas
Podemos manipular los valores del request antes de ser validados:
```php
public function all()
{
   $input = $this->all();
   if ($input['domain'] != '' && preg_match("#https?://#", $input['domain']) === 0) {
       $input['domain'] = 'http://'.$input['domain'];
   }

   $this->replace($input);
   return $input;
}
```
