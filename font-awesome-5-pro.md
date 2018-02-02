Al adquirir `Font Awesome 5 pro` nos comparten un token que nos sirve para instalar mediante NPM dicha versión. Por ejemplo:
> XD233A20-E440-EFGH-ABCD-7BCE0988CDF1

## Instalación
En consola ejecutamos:

    npm config set @fortawesome:registry https://npm.fontawesome.com/XD233A20-E440-EFGH-ABCD-7BCE0988CDF1
    npm i --save @fortawesome/fontawesome
    
Esta versión viene con 4 sabores de íconos, que son los siguientes:

    $ npm i --save @fortawesome/fontawesome-pro-solid
    $ npm i --save @fortawesome/fontawesome-pro-regular
    $ npm i --save @fortawesome/fontawesome-pro-light
    $ npm i --save @fortawesome/fontawesome-free-brands
    
En mi caso, me incliné por `fontawesome-pro-regular`, por lo tanto seguimos solo ejecuté el siguiente comando:

    npm i --save @fortawesome/fontawesome-pro-regular
    
## Usar en Laravel < 5.4
A partir de laravel 5.4 podemos compilar archivos de TypeScript. Súper últil para nuestro caso. 

### 1. Configurar webpack.mix
Esta configuración pretende preparar `webpack.mix` para que sepa con qué librería compilar y dónde se pueden encontrar las fuentes de TypeScript (.ts). Empezamos creando un archivo llamado `tsconfig.json` en la misma ubicación de `webpack.mix`, con el siguiente contenido:

```json
{
  "lib": [
    "dom",
    "es2016"
  ],
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "sourceMap": true
  },
  "include": [
    "resources/assets/ts/**/*"
  ]
}
```

> Nótese que hemos especificado que la carpeta `resources/assets/ts/` tendrá las fuentes de nuestros archivos .ts que compilaremos de ahora en adelante.

### 2. Agregar los íconos que nos interesan
Al menos en esta versión de Font Awesome uno debe "importar" los íconos que realmente se usarán. Si incluidmos lo más de 2000 que existen nuestra aplicación se tornara un poco pesada. Y tiene sentido, ya que en efecto, dificilmente usaremos todos los íconos en una aplicación. 

Dicho lo anterior, empezamos creando un archivo llamado `far.ts` dentro de `resources/assets/ts/`. Le he puesto ese nombre como abreviado de "Font Awesome Regular" para saber que en efecto son de tipo regular, pero se puede poner cualquier otro nombre. El contenido es el siguiente:

```typescript
import fontawesome from '@fortawesome/fontawesome'
import { faPlus, faUsers } from '@fortawesome/fontawesome-pro-regular'

fontawesome.library.add(faPlus);
fontawesome.library.add(faUsers);
```

Varias notas sobre el código:

* Las variables `faPlus` y `faUsers` son los nombres de los íconos según se definieron dentro de la librería de Font Awesome. Usan el mismo nombre pero en modo "camel case". Esta nomemclatura se debe respetar.
* Se pueden incluir tantos íconos como sea requerido
* Existe la variante de una sola línea, así: `fontawesome.library.add(faPlus, faUsers);`

### 3. Compilar
Ya sabemos que nuestras fuentes las compilamos con `npm run dev` o `npm run prod`. Pero antes, editamos el archivo `webpack.mix` y agregamos:

    mix.ts('resources/assets/ts/far.ts', 'js/fontawesome');
    
Esto dará como resultado el archivo `public/js/fontawesome/far.js`

### 4. HTML
Incluimos el archivo javascript:

```html
<script src="{{ asset('js/fontawesome/far.js') }}"></script>
```

Mostramos nuestro primer íconos (Nótese el nuevo prefijo `far`):

```html
<span class="far fa-users"></span>
```




