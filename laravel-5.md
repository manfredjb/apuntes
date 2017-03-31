## La clase Request
Clase de validación para los request enviados.

### Acceder los binds declarados en el Route
Carga el valor como un objeto si así el bind fue declarado de esa manera.
```php
$product = $request->route('product');
```

### Redirigir a otra página con un mensaje de error
Por defecto si la validación falla, el request regresa a la página de donde provino. Podemos hacer que redirija el error/mensajes a otra página:
```php
public function response(array $errors)
{
       return $this->redirector->route('buscador')
       ->withInput($this->except($this->dontFlash))
       ->withErrors($errors);
}
```

### Parsear los mensajes de error a un array de mensajes
Convierte una lista de errores a un objeto `MessageBag` que contiene los mensajes correspondientes
```php
use Illuminate\Support\MessageBag;
$errorBag = new MessageBag($errors);
```

### Emular la función sometimes()
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

### Sobreescribir mensajes de error
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

### Manipular valores antes de aplicar las reglas
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

## Paginador
Crear un paginador de registros.
```php
public function logs(RevisarLogs $solicitud, EntityManager $em){
       // total por pagina
       $totalPorPagina = 20;
       
       // pagina actual
       $pagina = $solicitud->get('page', 0);
       $pagina = ($pagina == 0)? ($pagina * $totalPorPagina) : ($pagina * $totalPorPagina) - $totalPorPagina;
       
       // total de registros
       $total = total_registros();

       // cargar los resultados paginados
       $qb = $em->createQueryBuilder();
       $qb->select(['a']);
       $qb->from(Log::class, 'a');
       $qb->setFirstResult($pagina);
       $qb->setMaxResults($totalPorPagina);
       $qb->orderBy('a.fecha', 'desc');
       $logs = $qb->getQuery()->getArrayResult();
       
       // crear el paginador
       $paginador = new LengthAwarePaginator(
              $logs, 
              $total, 
              $totalPorPagina, 
              $solicitud->get('page', 1)
       );
       $paginador->setPath(route('depuracion.logs'));

       ...
}
```

## Diccionario de errores
Lista de errores conocidos en Laravel y como evitarlos.

## MethodNowAllowedHttpException
Este error se presenta cuando se intenta ingresar a un route usando un Metodo diferente al establecido. Ejemplo, accesar la siguiente URL en el navegador:

> localhost/eliminar-item/8

```php
Route::post('eliminar-item/{id}', 'ControladorItem@eliminar');
```
