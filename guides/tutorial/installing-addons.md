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

Install the Mirage addon as follows:

```bash
ember install ember-cli-mirage
```

Our primary focus with mirage will be in the `config.js` file, which is where we can define our API endpoints and our data.
We will be following the [JSON-API specification](http://jsonapi.org/) which requires our data to be formatted a certain way.
Let's configure Mirage to send back our rentals that we had defined above by updating `mirage/config.js`.

Notice that in the first line of the function we set `this.namespace` to `/api`.
Setting namespace lets mirage know to only provide data for URL requests that start with `api`.
For example, the data we are providing below will be substituted when a request comes in at `/api/rentals`.

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

Mirage works by overriding the JavaScript code that makes network requests and instead returns the JSON you specify.
We should note that this means you will not see any network requests in your development tools but will instead see the JSON logged in your console.
Our update to `mirage/config.js` configures Mirage so that whenever Ember Data makes a GET request to `/api/rentals`, Mirage will return this JavaScript object as JSON and no network request is actually made.
We also specified a `namespace` of `/api` in our mirage configuration.
Without this change, navigation to `/rentals` in our application would conflict with Mirage.

In order for this to work, we need our application to default to making requests to the namespace of `/api`.
To do this, we want to generate an application adapter.
An [Adapter](../../models/customizing-adapters/) is an object that [Ember Data](../../models/) uses to determine how we communicate with our backend.
We will cover Ember Data in more detail later in this tutorial.
For now, let's generate an adapter for our application:

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

If you were running `ember serve` or `ember test --serve` in another shell, restart each of those servers to include Mirage in your build.

Note that at this point of the tutorial, the data is still provided by the `app/routes/rentals.js` file. We will make use of the mirage data we set up here in the upcoming section called [Using Ember Data](../ember-data/).
