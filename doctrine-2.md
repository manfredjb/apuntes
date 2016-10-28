#Modelos
Definición de modelos

##Estrategias de ID
Usando el valor de una secuencia llamada `SECUENCIA_ACTIVIDAD`:
```php
/**
 * @id
 * @var string
 * @GeneratedValue(strategy="SEQUENCE")
 * @SequenceGenerator(sequenceName="SECUENCIA_ACTIVIDAD", initialValue=1, allocationSize=100)
 * @column(type="string", name="codigo")
 */
protected $codigo;
```
> ¿Cómo crear una [secuencia](oracle.md#secuencias)?
