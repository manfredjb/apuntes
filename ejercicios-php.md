###Objetos
Ejercicios orientado a objetos y funciones de clases.

* Crear una clase llamada `Matemática` que contenga las siguientes funciones:
  1. `sumaPares(numero1, numero2)`: función que sume todos los números pares entre dos números dados.
  
    ```php
    // 20
    $suma = $matematica->sumaPares(2, 8);
    ```
    
  2. `mitades(array)`: función que devuelva un array con la mitad del valor de cada número dado.
  
    ```php
    // [1.5, 2, 5, 8.5]
    $mitades = $matematica->mitades([3, 4, 10, 17]);
    ```
  
  3. `buscarDecenas(numero1, numero2)`: función retorne una array que contenga todas las decenas entre dos números dados.
  
    ```php
    // [30, 40, 50, 60]
    $decenas = $matematica->buscarDecenas(30, 60);
    ```
  
  4. `primosEntre(numero1, numero2)`: función que devuelva un array de números primos entre dos números datos.
  
    ```php
    // [7, 11, 13, 17, 19]
    $primos = $matematica->primosEntre(7, 20);
    ```
    
  
###HTML
Ejercicios de cómo utilizar los elementos `<form>` e `<input>`, `<select>`.

1. Utilizando `GET`, crear un formulario que envíe los siguientes parámetros e imprimir los valores:
    
    - nombre: texto
    - edad: texto 
    - nacionalidad: selección de algún valor entre A, B, y C.
    - sexo: check, si es femenino o masculino
    - colores: opción múltiple donde se seleccione al menos 3 opciones
    
2. Utilizando `POST`, crear un formulario que envíe los siguientes parámetros:

    - numeros: texto que contiene una lista de números separados por coma
    - rangos:  texton que contiene dos números separados por coma
    
  Leer los valores, y utilizando la clase `Matematica` llamar a todas las funciones usando los valores de `numeros` y `rangos`
