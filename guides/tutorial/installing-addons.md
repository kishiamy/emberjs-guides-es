Ember tiene un rico ecosistema de complementos que pueden ser añadidos a sus proyectos.
Los complementos proporcionan una amplia gama de funcionalidades en su proyecto, a menudo ayudan a ahorrar tiempo y le permiten enfocarse en su proyecto.

Para encontrar estos complementos, visite el sitio web [Ember Observer](https://emberobserver.com/). Este sitio cataloga y clasifica los complementos de Ember que han sido publicados hacia npm y les asigna una calificación basada en múltiples criterios.

Para Super Rentals, tomaremos ventaja de dos complementos: [ember-cli-tutorial-style](https://github.com/toddjordan/ember-cli-tutorial-style) y [ember-cli-mirage](http://www.ember-cli-mirage.com/).

### ember-cli-tutorial-style

En lugar de que copie/pegue el código CSS para dar estilo a nuestra aplicación Super Rentals, hemos creado un complemento llamado [ember-cli-tutorial-style](https://github.com/ember-learn/ember-cli-tutorial-style) que instantáneamente añade CSS al tutorial.
El complemento genera un archivo llamado `ember-tutorial.css` y lo pone en el directorio `vendor` de nuestra aplicación super rentals.

El [directorio `vendor`](../../addons-and-dependencies/managing-dependencies/#toc_other-assets) en Ember es un directorio especial donde tú puedes incluir contenido que es compilado en nuestra aplicación.

Conforme Ember CLI se ejecuta, toma el archivo CSS `ember-tutorial` y lo pone en un archivo llamado `vendor.css`.
El archivo `vendor.css` es referenciado en `app/index.html`, haciendo los estilos disponibles en el runtime.

Podemos hacer ajustes de estilo adicionales a `vendor/ember-tutorial.css`, y los cambios tomarán efecto inmediatamente.

Ejecutar el siguiente comando apra instalar el complemento:

```bash
ember install ember-cli-tutorial-style
```

Aquí está la salida en terminal:

```bash
NPM: Installed ember-cli-tutorial-style
installing ember-cli-tutorial-style
  create public/assets/images/teaching.png
  create vendor/ember-tutorial.css
Installed addon package.
```

Los complementos de Ember, al ser paquetes npm, son instalados con el comando `ember install` en el directorio `node_modules`, y agrega una entrada en el fichero `package.json`. Asegúrese de reiniciar su servidor después de que haya instalado exitosamente el complemento; primero, presione `Ctrl+C` para detener el servidor, entonces, reinicie el servidor ejecutando `ember server`
de nuevo. Reiniciar el servidor incorporará el nuevo CSS y refrescando la ventana del navegador le mostrará esto:

![super rentals styled homepage](/images/installing-addons/styled-super-rentals-basic.png)

### ember-cli-mirage

[Mirage](http://www.ember-cli-mirage.com/) es una biblioteca cliente HTTP usada para pruebas de aplicaciones Ember.
Para el caso de este tutorial, usaremos mirage como nuestro origen de datos en lugar de un tradicional servidor backend.
Mirage nos permitirá crear datos falsos para trabajar mientras desarrollamos la aplicación e imitará a un API.
Los datos y los 'endpoints' que hemos configurado aquí entrarán en escena en el tutorial después, cuando comencemos a usar Ember Data para hacer peticiones al servidor.

Instalar el complemento Mirage con el siguiente comando:

```bash
ember install ember-cli-mirage
```

Nuestra primera tarea en mirage será en el fichero `config.js`, que es donde vamos a definir los endpoints de nuestra API y los datos de prueba.
Seguiremos la [especificación JSON-API](http://jsonapi.org/) que requiere que nuestros datos se encuentren formateados en un específico formato.
Configuremos Mirage para enviar de vuelta los datos de las propiedades del alquiler que hemos definido arriba actualizando el fichero `mirage/config.js`.

Note que en la primera línea de la función hemos definido `this.namespace` con el valor `/api`.
Ajustando el namespace le permitirá a mirage cómo proveer información únicamente a las peticiones que comiencen con `api`.
Por ejemplo, los datos que estamos ajustando abajo serán provisto cuando una petición entre por `/api/rentals`.

```javascript {data-filename="mirage/config.js" data-diff="+1,+2,+3,+4,+5,+6,+7,+8,+9,+10,+11,+12,+13,+14,+15,+16,+17,+18,+19,+20,+21,+22,+23,+24,+25,+26,+27,+28,+29,+30,+31,+32,+33,+34,+35,+36,+37,+38,+39,+40,+41,+42,+43,+44,+45,-46,-47,-48,-49,-50,-51,-52,-53,-54,-55,-56,-57,-58,-59,-60,-61,-62,-63,-64,-65,-66,-67,-68,-69,-70"}
export default function() {
  this.namespace = '/api';

  this.get('/rentals', function() {
    return {
      data: [{
        type: 'rentals',
        id: 'grand-old-mansion',
        attributes: {
          title: 'Grand Old Mansion',
          owner: 'Veruca Salt',
          city: 'San Francisco',
          category: 'Estate',
          bedrooms: 15,
          image: 'https://upload.wikimedia.org/wikipedia/commons/c/cb/Crane_estate_(5).jpg',
          description: "This grand old mansion sits on over 100 acres of rolling hills and dense redwood forests."
        }
      }, {
        type: 'rentals',
        id: 'urban-living',
        attributes: {
          title: 'Urban Living',
          owner: 'Mike Teavee',
          city: 'Seattle',
          category: 'Condo',
          bedrooms: 1,
          image: 'https://upload.wikimedia.org/wikipedia/commons/0/0e/Alfonso_13_Highrise_Tegucigalpa.jpg',
          description: "A commuters dream. This rental is within walking distance of 2 bus stops and the Metro."
        }
      }, {
        type: 'rentals',
        id: 'downtown-charm',
        attributes: {
          title: 'Downtown Charm',
          owner: 'Violet Beauregarde',
          city: 'Portland',
          category: 'Apartment',
          bedrooms: 3,
          image: 'https://upload.wikimedia.org/wikipedia/commons/f/f7/Wheeldon_Apartment_Building_-_Portland_Oregon.jpg',
          description: "Convenience is at your doorstep with this charming downtown rental. Great restaurants and active night life are within a few feet."
        }
      }]
    };
  });
}
export default function() {
 // These comments are here to help you get started. Feel free to delete them.

  /*
    Config (with defaults).

    Note: these only affect routes defined *after* them!
  */

  // this.urlPrefix = '';    // make this `http://localhost:8080`, for example, if your API is on a different server
  // this.namespace = '';    // make this `/api`, for example, if your API is namespaced
  // this.timing = 400;      // delay for each request, automatically set to 0 during testing

  /*
    Shorthand cheatsheet:

    this.get('/posts');
    this.post('/posts');
    this.get('/posts/:id');
    this.put('/posts/:id'); // or this.patch
    this.del('/posts/:id');

    http://www.ember-cli-mirage.com/docs/v0.3.x/shorthands/
  */
}
```

Mirage trabaja anulando el código Javascript que hace la petición de red y en su lugar retorna el JSON que usted especifique.
Nosotros deberíamos notar que esto significa que no verás ninguna petición de red en tus herramientas de desarrollo pero podrá ver en su lugar el JSON en la salida de su consola.
Los cambios realizados en el fichero `mirage/config.js` configura Mirage para que en cualquier momento que Ember Data haga una petición GET a `/api/rentals`, Mirage retornará este objeto JavaScript como JSON y ninguna petición de red es hecha actualmente.
También especificamos un `namespace` con `/api` en nuestra configuración de mirage.
Sin este cambio, la navegación a `/rentals` en nuestra aplicación causaría un conflicto con Mirage.

Ahora necesitamos que nuestra aplicación, por defecto, direccione las peticiones al namespace `/api`.
Para lograr esto, vamos a generar un adaptador llamado application.
Un [Adaptador](../../models/customizing-adapters/) es un objeto que [Ember Data](../../models/) utiliza para determinar cómo nos comunicaremos con nuestro backend.
Cubriremos Ember Data en detalles después en este tutorial.
Por ahora, generaremos un adaptador para nuestra aplicación:

```bash
ember generate adapter application
```

This adapter will extend the [`JSONAPIAdapter`](https://api.emberjs.com/ember-data/release/classes/JSONAPIAdapter) base class from Ember Data:

```javascript {data-filename="app/adapters/application.js" data-diff="+4"}
import DS from 'ember-data';

export default DS.JSONAPIAdapter.extend({
  namespace: 'api'
});
```

Si usted ejecutó `ember serve` o `ember test --serve` en otra terminal shell, reinícielos para refrescar los servidores e incluir Mirage en su compilado.

Note que en este punto, los datos aún son provistos por el fichero `app/routes/rentals.js`. Nosotros haremos uso de mirage data que hemos ajustado aquí en la siguiente sección llamada [Usando Ember Data](../ember-data/).
