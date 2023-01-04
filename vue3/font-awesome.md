Para integrar Font Awesome con Vue se deben llevar a cabo los siguientes pasos.

## Asignar la licencia (opcional)
Si se cuenta con licencia Pro, se debe crear un archivo en el root del proyecto llamado `.npmrc` con el siguiente contenido:

```
@fortawesome:registry=https://npm.fontawesome.com/
//npm.fontawesome.com/:_authToken=XY233A20-E440-50CD-B398-7BCE0988CDF1
```
> El valor de `_authToken` es el número de licencia obtenido en Font Awesome.


## Instalación de librerías
A través de NPM se deben instalar las librerías deseadas, en este caso se instalarán todas las Pro, además del componente Font Awesome para Vue:

```cmd
// Cambiar a conveniencia
npm install --save @fortawesome/pro-duotone-svg-icons@^6.2.0
npm install --save @fortawesome/pro-light-svg-icons@^6.2.0
npm install --save @fortawesome/pro-regular-svg-icons@^6.2.0
npm install --save @fortawesome/pro-solid-svg-icons@^6.2.0
npm install --save @fortawesome/pro-thin-svg-icons@^6.2.0
npm install --save @fortawesome/sharp-solid-svg-icons@^6.2.0

// Componente para Vue
npm install --save @fortawesome/vue-fontawesome@^3.0.1
```

## Importar iconos en la aplicación
Para ello se hace un archivo dedicado para declarar todos los íconos que se quieran usar:

```javascript
// iconos.js

import {library} from "@fortawesome/fontawesome-svg-core";

import {
    faCircle,
    faCircleCheck,

} from '@fortawesome/pro-light-svg-icons'

library.add(faCircle)
library.add(faCircleCheck)
```

Luego, en el archivo `main.js` se editan con los siguientes cambios:

```js
// Se importan los iconos
import "./iconos.js";

/* Se importa el componente de FontAwesomeIcon */
import { FontAwesomeIcon, FontAwesomeLayers } from '@fortawesome/vue-fontawesome'

// Se registra el componente en la aplicación
createApp(App)
    .component('font-awesome-icon', FontAwesomeIcon)
    .component('font-awesome-layers', FontAwesomeLayers)
    .mount('#app')
```

Finalmente, para renderizar un ícono solo se usa el componente `<font-awesome-icon />`, por ejemplo:
```html
<font-awesome-icon icon="fa-light fa-circle"/>
```
  
