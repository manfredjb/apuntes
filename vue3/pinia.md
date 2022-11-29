Para entender un poco [Pinia](https://pinia.vuejs.org), vamos a crear un objeto básico:

```js
import { defineStore } from 'pinia'

export const usarMiCasa = defineStore('casa', {
    state: () => ({
    
        /*
         * Uso de un valor escalar
         */
        niveles: 1,
        
        /*
         * Uso de valores como objeto
         */
        habitaciones: [],

        /*
         * Uso de valores como un contenedor asociada con un ID
         */
        habitantes: new Map(),
    }),
    
    /*
     * Los getters son como las acciones "computed" de un componente de Vue
     */
    getters: {
        /*
         * Método get de los niveles
         */
        cantidadNiveles: (state) => {
            return () => state.niveles;
        },
        
    },
    
    /*
     * Los actions son como los "methods" de un componente Vue
     */
    actions: {
        agregarHabitacion(numero, dimensiones, tieneVentana){
            this.habitaciones.push({
              numero: numero,
              dimensiones: dimensiones,
              tieneVentana: tieneVentana
            });
        },
        agregarHabitante(identificacion, datos){
            // Si existe, solo se actualiza la información, sino solo se agrega un nuevo registro
            this.lineas.add(identificacion, datos);
        }
    },
})
```

Para hacer una inclusión del store **casa** se procede con:

```js 
import { usarMiCasa } from "./MiCasa.js";

// Solo se hace un return dentro de los datos
data(){
  return {
    miCasa: new usarMiCasa()
  }
},
```

Llamar a una función getter:
```js 
let cantidad = miCasa.cantidadNiveles;
```

Llamar a una acción: 
```js 
miCasa.agregarHabitante(70175, {
  nombre: "Luz",
  nacionalidad: "CRC"
});
```

