Para mostrar la configuración básica de una aplicación Ember, vamos a construir una aplicación web para un sitio de alquiler de propiedades inmuebles llamado Super Rentals. Comenzaremos con una página de inicio (homepage), una página Acerca de(about) y una página de contacto.

Antes de comenzar, echemos un vistazo a lo que queremos contruir.

![super rentals homepage screenshot](/images/service/style-super-rentals-maps.png)

Primero, hagamos una lista de lo que queremos que haga la página de inicio. Queremos que nuestra aplicación:

* Muestre las propiedades de alquiler en la página de inicio.
* Enlace a información sobre la compañía.
* Enlace a la información de contacto.
* Listar las propiedades disponiles para alquier.
* Filtrar la lista de propiedades de alquiler por ciudad.
* Mostrar más detalles cuando se seleccione una propiedad.

Por el resto de esta página, le daremos una introducción a *testing* en Ember y prepararlo para añadir casos de prueba conforme implementamos más piezas a nuestra aplicación. En posteriores páginas del tutorial, la sección final de cada página será dedicada para añadir un caso de prueba de la característica que haya implementado en la aplicación. Estas mencionadas secciones no son requeridas en una aplicación funcional y usted podría seguir con el tutorial sin añadirlas.

En este punto, usted puede continuar a la [siguiente página](../routes-and-templates/) o leer más sobre casos de pruebas en Ember en la sección de abajo.

### Probando nuestra aplicación en el camino

Los objetivos listados en la sección anterior, podemos representarlos como [Ember application tests](../../testing/acceptance/). Las pruebas de aplicación (*Application tests*) interactúan con nuestra aplicación como una persona lo haría, pero automatizada, ayudando a asegurar que nuestra aplicación deje de funcionar conforme usted hace cambios.
Probablemente habrá escuchado referirse a estos tests como "acceptance tests" o pruebas de aceptación en Ember, y así es como han sido llamados en el pasado.

Cuando creamos un nuevo proyecto de Ember usando Ember CLI, este proyecto usa el [`QUnit`](https://qunitjs.com/) JavaScript test framework para definir y ejecutar tests.

Nosotros comenzaremos usando Ember CLI para generar un nuevo test de aplicación:

```bash
ember g acceptance-test list-rentals
```

El comando generará la siguiente salida en la consola, mostrando que se ha creado un único archivo llamado `tests/acceptance/list-rentals-test.js`.

```bash
installing acceptance-test
  create tests/acceptance/list-rentals-test.js
```

Al abrir el archivo podrá observar código generado que intenta ir a la ruta `list-rentals` y verifica que la ruta haya sido cargada. El código inicial nos ayudará a construir la primera prueba de aplicación.

Desde que no hemos añadido ninguna funcionalidad a nuestra aplicación aún, usaremos esta primera prueba para empezar a ejecutar pruebas en nuestra aplicación.

Para hacer eso, reemplazaremos todas las apariciones de `/list-rentals` en la prueba generada con `/`. La prueba iniciará nuestra aplicación en la URL base, `http://localhost:4200/`, y entonces hace una verificación básica donde la página ha terminado de cargar y la URL es la que deseamos que sea.

```javascript {data-filename="tests/acceptance/list-rentals-test.js" data-diff="-8,+9,-10,+11,-12,+13"}
import { module, test } from 'qunit';
import { visit, currentURL } from '@ember/test-helpers';
import { setupApplicationTest } from 'ember-qunit';

module('Acceptance | list rentals', function(hooks) {
  setupApplicationTest(hooks);

  test('visiting /list-rentals', async function(assert) {
  test('visiting /', async function(assert) {
    await visit('/list-rentals');
    await visit('/');
    assert.equal(currentURL(), '/list-rentals');
    assert.equal(currentURL(), '/');
  });
});
```

Algunas cosas que debemos resaltar en esta simple prueba:

* Pruebas de aplicación son configuradas usando la función `setupApplicationTest`. Esta función asegura que tu aplicación Ember ha iniciado y terminado en cada ejecución de test.
* QUnit pasa en un objeto llamado un [`assert`](https://api.qunitjs.com/assert/) a cada función de pruebas. Un `assert` tiene funciones tales como `equal()`, que permite a tu prueba evaluar mediante condiciones dentro del ambiente de pruebas. Una prueba debe pasar al menos un assert para ser exitoso.
* Las pruebas de una aplicación Ember usa un conjunto de functiones de ayuda tales como `visit` y `currentURL` usadas en el código de arriba. Discutiremos estas funciones en más detalle más adelante en el tutorial.

Ahora ejecutemos el conjunto de pruebas con el comando CLI, `ember test --server`.

Por defecto, cuando ejecuta el comando `ember test --server`, Ember CLI ejecuta el [Testem test runner](https://github.com/testem/testem), que a su vez ejecuta QUnit en el navegador Chrome.

Al mostrarse nuestro navegador web Chrome, muestra ahora 9 pruebas existosas. Si chequea el cuadro de texto con la etiqueta "Hide passed tests", usted debería ser capaz de ver nuestra prueba de aplicación exitosa, junto con 8 pruebas de tipo ESLint pasadas. Ember prueba cada archivo que haya creado para evaluar casos de sintaxis (conocido como "linting") usando [ESLint](http://eslint.org/).

![Captura de Pruebas Iniciales](/images/acceptance-test/initial-tests.png)

### Añadiendo Los Objetivos De Tu Aplicación como Texts

Como se mencionó antes, nuestra prueba inicial sólo se aseguró que todo estaba funcionando correctamente. Ahora reemplazaremos esa prueba con la lista de tareas que queremos que nuestra aplicación maneje(descrito arriba).

```javascript {data-filename="tests/acceptance/list-rentals-test.js" data-diff="+8,+9,+10,+11,+12,+13,+14,+15,+16,+17,+18,+19,+20,+21,+22,+23,+24,+25,-26,-27,-28,-29"}
import { module, test } from 'qunit';
import { visit, currentURL } from '@ember/test-helpers';
import { setupApplicationTest } from 'ember-qunit';

module('Acceptance | list-rentals', function(hooks) {
  setupApplicationTest(hooks);

  test('should show rentals as the home page', async function (assert) {
  });

  test('should link to information about the company.', async function (assert) {
  });

  test('should link to contact information.', async function (assert) {
  });

  test('should list available rentals.', async function (assert) {
  });

  test('should filter the list of rentals by city.', async function (assert) {
  });

  test('should show details for a selected rental', async function (assert) {
  });

  test('visiting /', async function(assert) {
    await visit('/');
    assert.equal(currentURL(), '/');
  });

});
```

Ejecutando el comando `ember test --server` la consola nos mostrará 7 pruebas fallando(de un total de 14). Cada uno de las 6 pruebas que configuramos arriba fallarán, además de una prueba de sintaxis ESLint que mostrará la siguiente falla `assert is defined but never used`. Las pruebas arriba fallan porque QUnit requiere que se verifique al menos una condición específica (conocida como `assert`).

Conforme avanzamos en este tutorial, usaremos las pruebas de aplicación como nuestra lista de objetivos a cumplir. Una vez que todas las pruebas han pasado, habremos cumplido con nuestros elevados objetivos.

![Captura de Pruebas Iniciales](/images/acceptance-test/acceptance-test.png)
 
