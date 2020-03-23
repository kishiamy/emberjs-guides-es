Ahora, vamos a añadir una lista de inmuebles disponibles para renta a la página rentals que ya habíamos creado.

Ember guarda los datos para una página en un objeto llamado `model`.
Para comenzar, llenaremos el modelo de nuestra página de propiedades en renta con un arreglo estático(hard-coded) de objetos Javascript.
Comenzaremos con algo sencillo,
llenaremos el modelo de nuestra página de propiedades en renta con un arreglo de objetos Javascript en el código (hard-coded).
Después, lo cambiaremos para usar [Ember Data](https://github.com/emberjs/data),
una librería para fortalecer el manejo de datos en nuestra aplicación.

Así se verá nuestra página de inicio cuado hayamos terminado:

![super rentals homepage with rentals list](/images/model-hook/super-rentals-index-with-list.png)

En Ember, los manejadores de ruta son los responsables de cargar el modelo con datos para la página.
Este carga los datos en una función llamada [`model`](https://api.emberjs.com/ember/3.11/classes/Route/methods/model?anchor=model/).
La función `model` actúa como un [hook](../../getting-started/core-concepts/#toc_hooks), lo que quiere decir que Ember lo llamará por nosotros en diferentes momentos en nuestra aplicación.
La functión de modelo que hemos añadido a nuestro manejador de ruta `rentals` será llamado cuando un usuario navegue a la ruta rentals vía URL raíz `http://localhost:4200`, o via `http://localhost:4200/rentals`.

Abramos el archivo `app/routes/rentals.js` y añadiremos una función `model` que retornará un arreglo de objetos de tipo rental:

```javascript {data-filename="app/routes/rentals.js" data-diff="+4,+5,+6,+7,+8,+9,+10,+11,+12,+13,+14,+15,+16,+17,+18,+19,+20,+21,+22,+23,+24,+25,+26,+27,+28,+29,+30,+31,+32,+33"}
import Route from '@ember/routing/route';

export default Route.extend({
  model() {
    return [{
      id: 'grand-old-mansion',
      title: 'Grand Old Mansion',
      owner: 'Veruca Salt',
      city: 'San Francisco',
      category: 'Estate',
      bedrooms: 15,
      image: 'https://upload.wikimedia.org/wikipedia/commons/c/cb/Crane_estate_(5).jpg',
      description: 'This grand old mansion sits on over 100 acres of rolling hills and dense redwood forests.'
    }, {
      id: 'urban-living',
      title: 'Urban Living',
      owner: 'Mike TV',
      city: 'Seattle',
      category: 'Condo',
      bedrooms: 1,
      image: 'https://upload.wikimedia.org/wikipedia/commons/0/0e/Alfonso_13_Highrise_Tegucigalpa.jpg',
      description: 'A commuters dream. This rental is within walking distance of 2 bus stops and the Metro.'
    }, {
      id: 'downtown-charm',
      title: 'Downtown Charm',
      owner: 'Violet Beauregarde',
      city: 'Portland',
      category: 'Apartment',
      bedrooms: 3,
      image: 'https://upload.wikimedia.org/wikipedia/commons/f/f7/Wheeldon_Apartment_Building_-_Portland_Oregon.jpg',
      description: 'Convenience is at your doorstep with this charming downtown rental. Great restaurants and active night life are within a few feet.'
    }];
  }
});
```

Usted puede notar que en el código estamos usando la sintaxis abreviada para definir métodos de ES6: `model()` es lo mismo que escribir `model: function()`.

Ember usará el objeto model retornado en el código de arriba y lo guardará en un atributo llamado `model`,
disponible para la plantilla rentals que generamos con nuestra ruta en [Rutas y Plantillas](../routes-and-templates/#toc_a-rentals-route).

Ahora, pasemos a nuestra plantilla de la página de alquiler de propiedades.
Nosotros podemos user el atributo model para mostrar nuestra lista de propiedades para rentar.
Aquí usaremos otro helper de Handlebars muy común llamado [`{{each}}`](../../templates/displaying-a-list-of-items/).
Este helper nos permitirá recorrer cada uno de los objetos rental en nuestro modelo:

```handlebars {data-filename="app/templates/rentals.hbs" data-diff="+12,+13,+14,+15,+16,+17,+18,+19,+20,+21,+22,+23,+24,+25,+26,+27,+28,+29,+30"}
<div class="jumbo">
  <div class="right tomster"></div>
  <h2>Welcome!</h2>
  <p>
    We hope you find exactly what you're looking for in a place to stay.
  </p>
  {{#link-to "about" class="button"}}
    About Us
  {{/link-to}}
</div>

{{#each this.model as |rental|}}
  <article class="listing">
    <div class="details">
      <h3>{{rental.title}}</h3>
      <div class="detail owner">
        <span>Owner:</span> {{rental.owner}}
      </div>
      <div class="detail type">
        <span>Type:</span> {{rental.category}}
      </div>
      <div class="detail location">
        <span>Location:</span> {{rental.city}}
      </div>
      <div class="detail bedrooms">
        <span>Number of bedrooms:</span> {{rental.bedrooms}}
      </div>
    </div>
  </article>
{{/each}}
```

En esta plantilla, recorremos cada uno de los objetos.
En cada iteración, el objeto actual queda guardado en una variable llamada `rental`.
De la variable rental en cada iteración, creamos una lista con información sobre la propiedad de alquiler.

Puede seguir a la [siguiente página](../installing-addons/) para seguir implementando nuevas características, o continuar leyendo sobre realizar pruebas a la app que ha creado.

### Pruebas de Aplicación a la Lista de Alquileres

Para revisar que las propiedades para alquiler son listadas con una prueba automatizada, tenemos que crear una prueba que visite la ruta index y revise que el resultado muestre 3 items en el listado.

En la plantilla `app/templates/rentals.hbs` rodeamos cada muestra del alquiler con un elemento `article` y le pusimos una clase css llamada `listing`.
Usaremos la clase CSS listing para averiguar cuantas propiedades de alquiler son mostradas en la página.


Para encontrar los elementos que tienen una clase CSS llamada `listing` usaremos el método [`querySelectorAll`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelectorAll).
El método `querySelectorAll` retorna los elementos que sean iguales al [selector CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors).
En este caso retornará un arreglo de todos los elementos con una clase CSS llamada `listing`.

```javascript {data-filename="tests/acceptance/list-rentals-test.js" data-diff="+2,+3"}
test('should list available rentals.', async function(assert) {
  await visit('/');
  assert.equal(this.element.querySelectorAll('.listing').length, 3, 'should display 3 listings');
});
```

Ejecuta las pruebas de nuevo usando el comando `ember t -s` y alterna el botón "Hide passed tests" para mostrar tus nuevas pruebas.

En este punto estamos lstando propiedades de alquiler y verificándolo con una prueba de aplicación.
Esto nos deja con 2 fallas restantes en las pruebas de aplicación (y 1 falla de ESLint):

![list rentals test passing](/images/model-hook/model-hook.png)
