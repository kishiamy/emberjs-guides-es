Para Super Rentals, queremos llegar a una página de inicio que muestre una lista de alquileres. A partir de ahí, deberíamos poder visitar una página sobre nosotros y nuestra página de contacto.

## Sobre la Ruta

Comencemos construyendo nuestra página "about".

En Ember, cuando queremos hacer una nueva página que se pueda visitar usando una URL, necesitamos generar una "ruta" usando Ember CLI. Para una descripción rápida de cómo Ember estructura las cosas, consulte [nuestro diagrama en la página Conceptos básicos] (https://guides.emberjs.com/release/getting-started/core-concepts/). Usemos el generador de rutas de Ember para comenzar nuestra ruta `about`.

```bash
ember generate route about
```

or para abreviar,

```bash
ember g route about
```

_Nota: Al ejecutar `ember help generate`, se enumerarán otros recursos de Ember que también puedes crear ..._ Y esto es lo que imprime nuestro generador:

```bash
installing route
  create app/routes/about.js
  create app/templates/about.hbs
updating router
  add route about
installing route-test
  create tests/unit/routes/about-test.js
```

Una ruta en Ember se construye con tres partes:

1. Una entrada en el enrutador Ember (`/app/router.js`), que se asigna entre nuestro nombre de ruta y un URI específico
2. Un archivo de controlador de ruta, que configura lo que debe suceder cuando se carga esa ruta _` (app/routes/about.js) `_
3. Una plantilla de ruta, que es donde mostramos el contenido real de la página _` (app/templates/about.hbs) `_

Si abrimos `/app/router.js`, veremos una nueva línea de código para la ruta ** about **, llamando `this.route ('about')` en la función `Router.map`. Esa nueva línea de código le dice al enrutador de Ember para ejecutar nuestro archivo `/app/routes/about.js` cuando un visitante navega a `/about`.


```javascript
import EmberRouter from '@ember/routing/router';
import config from './config/environment';

const Router = EmberRouter.extend({
  location: config.locationType,
  rootURL: config.rootURL
});

Router.map(function() {
  this.route('about');
});

export default Router;
```

Debido a que solo planeamos mostrar contenido estático en nuestra página acerca de (about), no ajustaremos el `/app/routes/about.js` enrutador de ruta ahora mismo. En cambio, abramos nuestro archivo de plantilla `/app/templates/about.hbs` y agreguemos información sobre Super Rentals:

```handlebars
<div class="jumbo">
  <div class="right tomster"></div>
  <h2>About Super Rentals</h2>
  <p>
    The Super Rentals website is a delightful project created to explore Ember.
    By building a property rental site, we can simultaneously imagine traveling
    AND building Ember applications.
  </p>
</div>
```

Ahora ejecute `ember serve` (o` ember server`, o incluso `ember s` para abreviar /) en su línea de comando para comenzar el servidor de desarrollo Ember y luego vaya a [`http://localhost:4200/about`] (http://localhost:4200/about/) para ¡ver nuestra nueva página en acción!

## Una ruta de Contacto

Ahora creemos otra ruta con los datos de contacto de la empresa. Una vez más, comenzaremos generando una ruta:

```bash
ember g route contact
```

Una vez más, agregamos una nueva ruta `contact` en` app/router.js` y generamos un controlador de ruta en `app/routes/contact.js`.

En la plantilla de ruta `/app/templates/contact.hbs`, agreguemos nuestros datos de contacto:

```handlebars
<div class="jumbo">
  <div class="right tomster"></div>
  <h2>Contact Us</h2>
  <p>Super Rentals Representatives would love to help you<br>choose a destination or answer
    any questions you may have.</p>
  <address>
    Super Rentals HQ
    <p>
      1212 Test Address Avenue<br>
      Testington, OR 97233
    </p>
    <a href="tel:503.555.1212">+1 (503) 555-1212</a><br>
    <a href="mailto:superrentalsrep@emberjs.com">superrentalsrep@emberjs.com</a>
  </address>
</div>
```

Ahora, cuando vayamos a [`http://localhost:4200/contact`] (http://localhost:4200/contact), veremos nuestra página de contacto.

## Navegando con enlaces y el Helper {{link-to}}

Moverse por nuestro sitio es un poco difícil en este momento, así que hagámoslo más fácil. Pondremos un enlace a la página de contacto en la página acerca de (about), y un enlace correspondiente a la página acerca de en la página de contacto.

Para hacer eso, usaremos un ayudante o helper [`{{link-to}}`] (https://guides.emberjs.com/release/templates/links/) que Ember proporciona para facilitar el enlace entre nuestras rutas. Vamos a ajustar nuestro archivo `about.hbs`:

```handlebars
<div class="jumbo">
  <div class="right tomster"></div>
  <h2>About Super Rentals</h2>
  <p>
    The Super Rentals website is a delightful project created to explore Ember.
    By building a property rental site, we can simultaneously imagine traveling
    AND building Ember applications.
  </p>
  {{#link-to "contact" class="button"}}
    Contact Us
  {{/link-to}}
</div>
```

En este caso, le estamos diciendo al helper `{{link-to}}` el nombre de la ruta a la que queremos vincular: `contact`. Cuando miramos nuestra página 'acerca de' en [`http://localhost:4200/about`] (http://localhost:4200/about), ahora tenemos un enlace de trabajo a nuestra página de contacto:

![super rentals about page screenshot](/images/routes-and-templates/ember-super-rentals-about.png)

Ahora, agregaremos nuestro enlace correspondiente a la página de contacto para que podamos avanzar y retroceder entre `about` y` contact`:

```handlebars
<div class="jumbo">
  <div class="right tomster"></div>
  <h2>Contact Us</h2>
  <p>Super Rentals Representatives would love to help you<br>choose a destination or answer
    any questions you may have.</p>
  <address>
    Super Rentals HQ
    <p>
      1212 Test Address Avenue<br>
      Testington, OR 97233
    </p>
    <a href="tel:503.555.1212">+1 (503/) 555-1212</a><br>
    <a href="mailto:superrentalsrep@emberjs.com">superrentalsrep@emberjs.com</a>
  </address>
  {{#link-to "about" class="button"}}
    About
  {{/link-to}}
</div>
```

## La Ruta de alquileres

Además de nuestras páginas `about` y` contact`, queremos mostrar una lista de alquileres que nuestros visitantes pueden mirar a través. Entonces agreguemos una tercera ruta y llamémosla `rentals`:

```bash
ember g route rentals
```

Y luego vamos a actualizar nuestra nueva plantilla (`/app/templates/rentals.hbs`) con algo de contenido inicial. Regresaremos a esta página en un momento para agregar las propiedades de alquiler reales.

```handlebars
<div class="jumbo">
  <div class="right tomster"></div>
  <h2>Welcome!</h2>
  <p>We hope you find exactly what you're looking for in a place to stay.</p>
  {{#link-to "about" class="button"}}
    About Us
  {{/link-to}}
</div>
```

## La Ruta Índice

Con nuestras tres rutas en su lugar, estamos listos para agregar una ruta de índice, que manejará las solicitudes al URI raíz (`/`) de nuestro sitio. Nos gustaría hacer que la página de alquileres sea la página principal de nuestra aplicación, y ya hemos creado una ruta. Por lo tanto, queremos que nuestra ruta de índice simplemente reenvíe a la ruta de `rentals` que ya hemos creado.

Usando el mismo proceso que hicimos para nuestras páginas acerca de y de contacto, primero generaremos una nueva ruta llamada `index`.

```bash
ember g route index
```

Ahora podemos ver el resultado familiar para el generador de ruta:

```bash
installing route
  create app/routes/index.js
  create app/templates/index.hbs
installing route-test
  create tests/unit/routes/index-test.js
```

A diferencia de los otros controladores de ruta que hemos hecho hasta ahora, la ruta `index` es especial: NO requiere una entrada en la asignación del enrutador. Aprenderemos más acerca de por qué no se requiere la entrada más adelante cuando veamos [rutas anidadas] (../subroutes/) en Ember.

Todo lo que queremos hacer cuando un usuario visita la URL raíz (`/`) es la transición a `/rentals`. Para hacer esto, agregaremos código a nuestro controlador de ruta de índice mediante la implementación de un enlace de ciclo de vida de ruta llamado `redirect`. Los enganches del ciclo de vida de la ruta son métodos especiales que se llaman automáticamente cuando una ruta se procesa o los datos cambian. En el interior, llamaremos a la función transitionTo:

```javascript
import Route from '@ember/routing/route';

export default Route.extend({
  redirect() {
    this.transitionTo('rentals');
  }
});
```
Ahora, visitando la ruta raíz en `/` dará como resultado la carga de URL `/rentals`.

## Agregando un banner con navegación

Además de agregar enlaces individuales a cada ruta de nuestra aplicación, nos gustaría agregar un encabezado común en la parte superior de nuestra página para mostrar el título de nuestra aplicación y su barra de navegación.

Para mostrar algo en cada página, podemos usar la plantilla de la aplicación (que editamos anteriormente). Volvamos a abrirlo (`/app/templates/application.hbs`) y reemplace su contenido con lo siguiente:

```
<div class="container">
  <div class="menu">
    {{#link-to "index"}}
      <h1>
        <em>SuperRentals</em>
      </h1>
    {{/link-to}}
    <div class="links">
      {{#link-to "about" class="menu-about"}}
        About
      {{/link-to}}
      {{#link-to "contact" class="menu-contact"}}
        Contact
      {{/link-to}}
    </div>
  </div>
  <div class="body">
    {{outlet}}
  </div>
</div>
```

Hemos visto la mayoría de esto antes, pero el `{{outlet}}` debajo de `<div class =" body ">` es nuevo. El ayudante `{{outlet}}` le dice a Ember dónde debe mostrarse el contenido de nuestra ruta actual (como `about` o` contact`).

En este punto, deberíamos poder navegar entre nuestras páginas `acerca de`,` contacto` y `alquileres`.

Desde aquí puede pasar a la [página siguiente] (../model-hook/) o sumergirse en la prueba de la nueva funcionalidad que acabamos de agregar.

## Implementando pruebas de aplicación

Ahora que tenemos varias páginas en nuestra aplicación, veamos cómo crear pruebas para ellas.

Como se mencionó anteriormente en la página [Planificación de la aplicación] (../acceptance-test), una prueba de aplicación Ember automatiza la interacción con nuestra aplicación de manera similar a la de un visitante.

Si abre la prueba de aplicación que creamos (`/tests/accept/list-rentals-test.js`), verá nuestros objetivos, que incluyen la capacidad de navegar a una página `acerca de` y una página `contacto`. Comencemos por ahí.

Primero, queremos probar que visitar `/` redirige correctamente a `/rentals`. Utilizaremos el ayudante `visit` de Ember y luego nos aseguraremos de que nuestra URL actual sea `/rentals` una vez que ocurra la redirección.

```javascript
import { module, test } from 'qunit';
import { visit, currentURL } from '@ember/test-helpers';
import { setupApplicationTest } from 'ember-qunit';

module('Acceptance | list rentals', function (hooks) {
  setupApplicationTest(hooks);

  test('should show rentals as the home page', async function (assert) {
    await visit('/');
    assert.equal(currentURL(), '/rentals', 'should redirect automatically');
  });
});
```

Ahora ejecute las pruebas escribiendo `ember test --server` en la línea de comando (o `ember t -s` para abreviar).

En lugar de 7 fallas, ahora debería haber 6 (5 fallas de aplicación y 1 ESLint). También puede ejecutar nuestra prueba específica seleccionando la entrada llamada "Aceptación | alquileres de lista" en la entrada desplegable etiquetada "Módulo" en la interfaz de usuario de prueba.

También puede alternar "Ocultar pruebas aprobadas" para mostrar su caso de prueba aprobada junto con las pruebas que todavía están fallando (porque aún no las hemos creado).

![6_fail](../../images/routes-and-templates/routes-and-templates.gif)

### Testeando helpers de Ember

Ember ofrece una variedad de helpers de test de aplicaciones para facilitar las tareas comunes, como visitar rutas, completar campos, hacer clic en enlaces/botones y esperar a que se muestren las páginas.

Algunos de los ayudantes que usaremos comúnmente son:

* [`visit`] (https://github.com/emberjs/ember-test-helpers/blob/master/API.md#visit): carga una URL determinada
* [`click`] (https://github.com/emberjs/ember-test-helpers/blob/master/API.md#click) - pretende ser un usuario haciendo clic en una parte específica de la pantalla
* [`currentURL`] (https://github.com/emberjs/ember-test-helpers/blob/master/API.md#currenturl): devuelve la URL de la página en la que estamos actualmente

Importemos estos helpers a nuestra prueba de aplicación:

```javascript
import {
  click,
  currentURL,
  visit
} from '@ember/test-helpers';
```

### Test visitando nuestras páginas About y Contact

Ahora agreguemos un código que simule que un visitante llega a nuestra página de inicio, hace clic en uno de nuestros enlaces y luego visita una nueva página.

```javascript
test('should link to information about the company', async function(assert) {
  await visit('/');
  await click(".menu-about");
  assert.equal(currentURL(), '/about', 'should navigate to about');
});

test('should link to contact information', async function(assert) {
  await visit('/');
  await click(".menu-contact");
  assert.equal(currentURL(), '/contact', 'should navigate to contact');
});
```

En las pruebas anteriores, estamos utilizando [`assert.equal()`] (https://api.qunitjs.com/assert/equal) para verificar si el primer y el segundo argumento son iguales. Si no lo hacen, nuestra prueba fallará.

El tercer argumento opcional nos permite proporcionar un mensaje más agradable que se mostrará si esta prueba falla.

En nuestras pruebas, también llamamos a dos ayudantes (`visit` y` click`) uno tras otro. Aunque Ember hace una serie de cosas cuando hacemos esas llamadas, Ember oculta esas complejidades al darnos estos [ayudantes de prueba asincrónicos] (../../testing/accept/#toc_asynchronous-helpers).

Si dejaste `ember test` en ejecución, debería haberse actualizado automáticamente para mostrar que las tres pruebas relacionadas con la navegación ya han pasado.

En la grabación de pantalla a continuación, ejecutamos las pruebas, deseleccionamos "Ocultar pruebas aprobadas" y configuramos el módulo para nuestra prueba de aplicación, revelando las 3 pruebas que pasamos.

![pasando tests de navegación](../../images/routes-and-templates/ember-route-tests.gif)
