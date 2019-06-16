Esta guía rápida le enseñará cómo construir una aplicación Ember desde cero.

Los pasos a seguir son los siguientes:

1. Instalar Ember.
2. Crear una nueva aplicación.
3. Definir una ruta.
4. Añadir un componente.
5. Compilar su aplicación para desplegar a producción.

## Instalar Ember

Instalar Ember es muy fácil usando npm, el gestionador de paquetes de Node.js.
En su terminal ejecute el siguiente comando:

```bash
npm install -g ember-cli
```

¿No tiene instalado npm? [En este enlace aprenda cómo instalar Node.js y npm](https://docs.npmjs.com/getting-started/installing-node).
Encontrará una lista completa de las dependencias necesarias para un proyecto de Ember CLI en nuestra guía [Instalar Ember](../../getting-started/).

## Crear una Nueva aplicación

Si ha instalado el Ember CLI via npm,
en su terminal tendrá acceso a un nuevo comando llamado `ember`.
Usted puede usar el comando `ember new` para crear una nueva aplicación.

```bash
ember new ember-quickstart
```

Este comando se encargará de crear un nuevo directorio llamado `ember-quickstart` y dentro encontrará una nueva aplicación Ember.
Su aplicación incluirá:

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
¡Felicitaciones! Ha creado e iniciado su primera aplicación Ember.

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
Por ahora, puede considerar a las rutas como las diferentes páginas que forman parte de su aplicación.

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

Abra la recien creada plantilla `app/templates/scientists.hbs` y añada el siguiente código HTML:

```handlebars {data-filename=app/templates/scientists.hbs}
<h2>List of Scientists</h2>
```

En su navegador, abra [`http://localhost:4200/scientists`](http://localhost:4200/scientists).
Usted podrá observar el título `<h2>` que puso en la plantilla `scientists.hbs`,
justo debajo del título `<h1>` de su plantilla `application.hbs`.

Ahora que tenemos la plantilla `scientists` mostrándose en nuestro navegador,
vamos a incluir algo de datos para mostrar.
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
Para crear el nuevo componente use el siguiente comando:

```bash
ember generate component people-list
```

Ahora copie y pegue el contenido de la plantilla `scientists` que creamos dentro de la plantilla del componente `people-list` creado en el paso anterior y edite como se muestra a continuación:

```handlebars {data-filename=app/templates/components/people-list.hbs}
<h2>{{this.title}}</h2>

<ul>
  {{#each this.people as |person|}}
    <li>{{person}}</li>
  {{/each}}
</ul>
```

Note que hemos cambiado el título, de una cadena de texto "List of Scientists" a una propiedad dinámica llamada (`{{title}}`).
También hemos cambiado `scientist` a algo más genérico: `person`,
para facilitar su implementación en el proyecto.

Guarde la plantilla del componente y regrese a la plantilla `scientists`.
Reemplace todo el código de listado con esta versión que usa el componente creado.

Al componente vamos a enviarle:

1. El título que usará por medio del atributo `title`.
2. El arreglo con los datos de personas por medio del atributo `people`. Usaremos este modelo como el listado de personas.

```handlebars {data-filename="app/templates/scientists.hbs" data-diff="-1,-2,-3,-4,-5,-6,-7,+8"}
<h2>List of Scientists</h2>

<ul>
  {{#each this.model as |scientist|}}
    <li>{{scientist}}</li>
  {{/each}}
</ul>
<PeopleList @title="List of Scientists" @people={{this.model}} />
```

Vuelva a su navegador y verá que la interfaz de usuario parece no haber cambiado.
La única diferencia es que ahora tenemos un componente reusable y su código es más fácil de mantener.

Como ejercicio para usted,
intente crear una ruta nueva llamada `programmers` que muestre una lista de programadores famosos.
Usando el componente `people-list` lo logrará sin escribir código en absoluto.

## Evento Click

En este punto, su aplicación está mostrando un listado con datos,
pero no hay manera que el usuario pueda interactuar con la información mostrada.
En las aplicaciones web es común que desee escuchar los eventos del usuarios como clicks o hovers.
Ember facilita esta tarea.
Primero añada un ayudante de tipo `action` al elemento `li` en su componente `people-list`.

```handlebars {data-filename="app/templates/components/people-list.hbs" data-diff="-5,+6"}
<h2>{{this.title}}</h2>

<ul>
  {{#each this.people as |person|}}
    <li>{{person}}</li>
    <li {{action "showPerson" person}}>{{person}}</li>
  {{/each}}
</ul>
```

El ayudante de tipo `action` le permitirá añadir oyentes (listeners) de eventos e invocar functiones personalizadas.
Por defecto, el ayudante de tipo `action` añade un oyente de eventos llamado `click`,
pero puede agregar otros oyentes para cualquier evento del elemento.
Ahora, queremos que cuando el elemento `li` sea clickeado una función sea llamada del objeto `actions` en el componente `people-list`.
Imagine que lo que queremos lograr sería algo como `this.actions.showPerson(person)` desde nuestra plantilla.

Para invocar esta función usted necesitará modificar el componente llamado `people-list` y añadir la función que será invocada.
En el componente, añada un objeto de tipo `actions` con una función llamada `showPerson` que mostrará un cuadro de alerta con el primer argumento que recibe.

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

Ahora en el navegador, cuando el usuario haga click sobre el nombre de un científico,
esta función será invocada para mostrar el nombre de la persona en un cuadro de alerta.

## Compilando para producción

Ahora que hemos escrito nuestra aplicación y comprobado que funciona en desarrollo, es el momento de prepararse para implantarlo en nuestros usuarios. Para ello, ejecutamos el siguiente comando:

```bash
ember build --env production
```

El comando `build` empaqueta todos los recursos para preparar su aplicacion&mdash; JavaScript, plantillas, CSS, fuentes, imágenes y mucho más.

En este caso, le decimos a Ember que compile para el entorno de producción for medio de la directiva `--env`. Esto crea un set optimizado listo para subir al servidor de su web. Una vez la compilación termine, encontrará todos los ficheros compilados, ya concatenados y minimizados en su directorio `dist/` de la aplicación.

La comunidad Ember valora la colaboración y la construcción de herramientas comunes en las que todos confían. Si está interesado en implementar su aplicación en producción de forma rápida y confiable, eche un vistazo al complemento [Ember CLI Deploy] (http://ember-cli-deploy.com/).

Si implementa su aplicación en un servidor web Apache, primero cree un nuevo servidor virtual para la aplicación. Para asegurarse de que todas las rutas sean manejadas por index.html, agregue la siguiente directiva a la configuración de servidor virtual de la aplicación:

```apacheconf
FallbackResource index.html
```
