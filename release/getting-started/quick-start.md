Esta guía rápida le enseñará cómo construir una aplicación Ember desde cero.

Los pasos a seguir son los siguientes:

1. Instalar Ember.
2. Crear una nueva aplicación.
3. Definir una ruta.
4. Añadir un componente.
5. Compilar tu aplicación para desplegar a producción.

## Instalar Ember

Instalar Ember es muy fácil usando npm, el gestionador de paquetes de Node.js.
En su terminal ejecute el siguiente comando:

```bash
npm install -g ember-cli
```

No tiene instalado npm? [En este enlace aprenda cómo instalar Node.js y npm](https://docs.npmjs.com/getting-started/installing-node).
Encontrará una lista completa de las dependencias necesarias para un proyecto de Ember CLI en nuestra guía [Instalar Ember](../../getting-started/).

## Crear una Nueva aplicación

Si ha instalado el Ember CLI via npm,
en su terminal tendrá acceso a un nuevo comando llamado `ember`.
Usted puede usar el comando `ember new` para crear una nueva aplicación.

```bash
ember new ember-quickstart
```

Este comando se encargará de crear un nuevo directorio llamado `ember-quickstart` y dentro encontrará una nueva aplicación Ember.
Tu aplicación incluirá:

* Un servidor de desarrollo.
* Compilador de plantillas.
* Minificador de JavaScript y CSS.
* Uso de características ES2015 via Babel.

Comenzar nuevos proyectos con Ember es sencillo porque proporciona un paquete integrado con todo lo que necesita para construir una aplicación web de producción.

Comprobemos que todo está trabajando correctamente.
`cd` en el directorio de la aplicación `ember-quickstart` e inicie el servidor de desarrollo ejecutando los siguientes comandos:

```bash
cd ember-quickstart
ember serve
```

Después de unos pocos segundos debería mostrarse el siguente mensaje en la terminal:

```text
Livereload server on http://localhost:7020
Serving on http://localhost:4200/
```

(Para detener el servidor, presione Ctrl-C en su terminal.)

Abra [`http://localhost:4200`](http://localhost:4200) en el navegador preferido.
Debería ver una página de bienvenida de Ember.
¡Felicitaciones! Ha creado e iniciado tu primera aplicación Ember.

Ahora vamos a comenzar a editar la plantilla principal llamada `application`.
Esta plantilla se encuentra siempre presente en la pantalla mientras el usuario tenga cargada la aplicación Ember.
En su editor abra el archivo `app/templates/application.hbs` y edite con el siguiente contenido:

```handlebars {data-filename=app/templates/application.hbs}
<h1>PeopleTracker</h1>

{{outlet}}
```

Ember detecta los cambios realizados en los archivos de la aplicación  y automáticamente recarga la página en el navegador.
Usted debería ver que la página de bienvenida ha sido reemplazada por "PeopleTracker".
Recuerde añadir `{{outlet}}` a esa página,
que significa que toda ruta anidada será mostrada en ese lugar.

## Definir una ruta

Ahora añadiremos a la aplicación una lista de científicos.
El primer paso para hacerlo es crear una ruta.
Por ahora, puede considerar a las rutas como las diferentes páginas que forman parte de tu aplicación.

Ember incluye generadores que automatizan el código repetitivo para las tareas comunes.
Para generar una ruta, ejecute el siguiente comando en una nueva ventana de terminal desde el directorio de su aplicación `ember-quickstart`:

```bash
ember generate route scientists
```

Después de la ejecución se mostrará el siguiente mensaje en la terminal:

```text
installing route
  create app/routes/scientists.js
  create app/templates/scientists.hbs
updating router
  add route scientists
installing route-test
  create tests/unit/routes/scientists-test.js
```

Lo que se muestra es el detalle de lo que Ember ha creado:

1. Una plantilla para mostrar cuando el usuario visita la ruta `/scientists`.
2. Un objeto `Route` que recupera el modelo usado por la plantilla.
3. Una entrada con la ruta en el fichero que maneja las rutas de la aplicación (localizado en `app/router.js`).
4. Un fichero de prueba unitaria para esta ruta.

Abra la reciente plantilla creada `app/templates/scientists.hbs` y añada el siguiente código HTML:

```handlebars {data-filename=app/templates/scientists.hbs}
<h2>List of Scientists</h2>
```

En su navegador, abra [`http://localhost:4200/scientists`](http://localhost:4200/scientists).
Usted podrá observar el título `<h2>` que puso en la plantilla `scientists.hbs`,
justo debajo del título `<h1>` de su plantilla `application.hbs`.

Ahora que tenemos la plantilla `scientists` mostrándose en nuestro navegador,
vamos a incluir algo que datos para mostrar.
Lo lograremos especificando un modelo (_model_) para esta ruta,
y podemos especificar el modelo editando el fichero `app/routes/scientists.js`.

Tomaremos el código que hemos generado y añadiremos un método `model()` al objeto `Route`:

```javascript {data-filename="app/routes/scientists.js" data-diff="+4,+5,+6"}
import Route from '@ember/routing/route';

export default Route.extend({
  model() {
    return ['Marie Curie', 'Mae Jemison', 'Albert Hofmann'];
  }
});
```

Este ejemplo usa las características más recientes de Javascript, y es posible encuentre algunas con las que no se encuentre familiarizado.
Aprenda más con esta [descripción general de las nuevas funciones de Javascript](https://ponyfoo.com/articles/es6).

En el método `model()` de un objeto Route, usted puede retornar los datos que desee se encuentre disponibles en la plantilla.
Si usted necesita recuperar datos asincrónicamente,
el método `model()` soporta cualquier librería que use [JavaScript Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).

Ahora vamos a hacer que Ember convierta ese arreglo de cadenas de texto en HTML.
Abra la plantilla `scientists` y añada el siguiente código para recorrer el arreglo e imprimir su contenido.

```handlebars {data-filename="app/templates/scientists.hbs" data-diff="+3,+4,+5,+6,+7"}
<h2>List of Scientists</h2>

<ul>
  {{#each this.model as |scientist|}}
    <li>{{scientist}}</li>
  {{/each}}
</ul>
```

En el fragmento de código hemos usado la instrucción `each` para recorrer cada elemento en el arreglo que retornamos con el hook `model()` y lo imprimimos dentro de un elemento `<li>`.

## Crear un componente de interfaz de usuario

A medida que su aplicación crece, se dará cuenta que existe lógica de interfaz de usuario siendo compartida en varias páginas, 
o usándola muchas veces en la misma página.
Con Ember es muy sencillo transformar la lógica de tus plantillas en componentes reusables.

Vamos a crear un componente llamado `people-list` que podrá ser usado en distintos lugares de las plantillas para mostrar una lista de personas.

Como es usual, existe un generador que nos ayudará en esta tarea.
Make a new component by typing:

```bash
ember generate component people-list
```

Copy and paste the `scientists` template into the `people-list` component's template and edit it to look as follows:

```handlebars {data-filename=app/templates/components/people-list.hbs}
<h2>{{this.title}}</h2>

<ul>
  {{#each this.people as |person|}}
    <li>{{person}}</li>
  {{/each}}
</ul>
```

Note that we've changed the title from a hard-coded string ("List of Scientists") to a dynamic property (`{{title}}`).
We've also renamed `scientist` to the more-generic `person`,
decreasing the coupling of our component to where it's used.

Save this template and switch back to the `scientists` template.
Replace all our old code with our new componentized version.

We're going to tell our component:

1. What title to use, via the `title` attribute.
2. What array of people to use, via the `people` attribute. We'll
   provide this route's `model` as the list of people.

```handlebars {data-filename="app/templates/scientists.hbs" data-diff="-1,-2,-3,-4,-5,-6,-7,+8"}
<h2>List of Scientists</h2>

<ul>
  {{#each this.model as |scientist|}}
    <li>{{scientist}}</li>
  {{/each}}
</ul>
<PeopleList @title="List of Scientists" @people={{this.model}} />
```

Go back to your browser and you should see that the UI looks identical.
The only difference is that now we've componentized our list into a version that's more reusable and more maintainable.

You can see this in action if you create a new route that shows a different list of people.
As an exercise for the reader,
you may try to create a `programmers` route that shows a list of famous programmers.
By re-using the `people-list` component, you can do it in almost no code at all.

## Click Events

So far, your application is listing data,
but there is no way for the user to interact with the information.
In web applications you often want to listen for user events like clicks or hovers.
Ember makes this easy to do.
First add an `action` helper to the `li` in your `people-list` component.

```handlebars {data-filename="app/templates/components/people-list.hbs" data-diff="-5,+6"}
<h2>{{this.title}}</h2>

<ul>
  {{#each this.people as |person|}}
    <li>{{person}}</li>
    <li {{action "showPerson" person}}>{{person}}</li>
  {{/each}}
</ul>
```

The `action` helper allows you to add event listeners to elements and call named functions.
By default, the `action` helper adds a `click` event listener,
but it can be used to listen for any element event.
Now, when the `li` element is clicked a `showPerson` function will be called from the `actions` object in the `people-list` component.
Think of this like calling `this.actions.showPerson(person)` from our template.

To handle this function call you need to modify the `people-list` component file to add the function to be called.
In the component, add an `actions` object with a `showPerson` function that alerts the first argument.

```javascript {data-filename="app/components/people-list.js" data-diff="+4,+5,+6,+7,+8"}
import Component from '@ember/component';

export default Component.extend({
  actions: {
    showPerson(person) {
      alert(person);
    }
  }
});
```

Now in the browser when a scientist's name is clicked,
this function is called and the person's name is alerted.

## Building For Production

Now that we've written our application and verified that it works in development,
it's time to get it ready to deploy to our users.

To do so, run the following command:

```bash
ember build --env production
```

The `build` command packages up all of the assets that make up your
application&mdash;JavaScript, templates, CSS, web fonts, images, and
more.

In this case, we told Ember to build for the production environment via the `--env` flag.
This creates an optimized bundle that's ready to upload to your web host.
Once the build finishes,
you'll find all of the concatenated and minified assets in your application's `dist/` directory.

The Ember community values collaboration and building common tools that everyone relies on.
If you're interested in deploying your app to production in a fast and reliable way,
check out the [Ember CLI Deploy](http://ember-cli-deploy.com/) addon.

If you deploy your application to an Apache web server, first create a new virtual host for the application.
To make sure all routes are handled by index.html,
add the following directive to the application's virtual host configuration:

```apacheconf
FallbackResource index.html
```

