Antes de que comience a escribir código Ember, es buena idea revisar brevemente cómo funciona una aplicación Ember.

![Conceptos del núcleo](/images/ember-core-concepts/ember-core-concepts.png)

## Ruteadores y Manejadores de Rutas
Imagine que estamos escribiendo una aplicación web para un sitio que permite a los usuarios listar viviendas para rentar. En algún momento, nosotros deberíamos ser capaces de responder preguntas sobre su estado actual como _¿Qué vivienda están buscando?_ y _¿Están editándola?_ En Ember, la respuesta a esas preguntas es determinada por la URL.
La URL puede ser definida en las siguientes formas:

* El usuario carga la aplicación por primera vez.
* El usuario cambia el URL manualmente, haciendo click en el botón Atrás o editando la barra de dirección del navegador.
* El usuario hace click en un enlace dentro de la aplicación.
* Algunos otros eventos en la aplicación causan cambios de la URL.

No importa cómo la URL es definidda, la primera cosa que pasará es que el ruteador mapea el URL a un manejador de rutas.

El manejador de la ruta normalmente hace dos cosas:

* Renderiza una plantilla.
* Carga un modelo que estará disponible para la plantilla.

## Plantillas

Ember usa plantillas para organizar el diseño HTML en una aplicación.

La mayoría de plantillas con código base de Ember resultan muy familiares, y se parecen a cualquier otro fragmento HTML. Por ejemplo:

```handlebars
<div>Hola, esta es una plantilla Ember válida!</div>
```

Las plantillas de Ember usan la sintaxis de plantillas [Handlebars](http://handlebarsjs.com). Cualquier cosa que sea válido en Handlebars será válido en la sintaxis de las plantillas de Ember.

Las plantillas también pueden mostrar propiedades proporcionadas desde su contexto, ya sea desde un componente o un controlador de rutas. Por ejemplo:

```handlebars
<div>Hola {{this.name}}, esta es una plantilla Ember válida</div>
```

En este ejemplo, `{{this.name}}` es una propiedad proporcionada por el contexto de la plantilla.

Además de propiedades, la notación de doble llaves (`{{}}`) podría contener helpers o ayudantes, de los cuales hablaremos más adelante.

## Modelos

Los modelos representan estado de persistencia.

Por ejemplo, una aplicación para alquiler de viviendas querría guardar los detalles de una vivienda cuando un usuario lo publica, entonces la vivienda tendría un modelo con la definición de dichos detalles, tal vez el modelo podría llamarse _rental_.

Un modelo normalmente persiste información a un servidor web, sin embargo los modelos pueden configurarse para guardar información en otros lugares, como por ejemplo: el almacenamiento local del navegador (Local Storage).

## Componentes

Mientras las plantillas describen cómo se ve la interfaz de usuario, los componentes controlan el _comportamiento_ de la interfaz de usuario.

Los componentes consisten en dos partes: una plantilla escrita en Handlebars, y un archivo fuente escrito en Javascript que define el comportamiento del componente. Por ejemplo: nuestra aplicación podría tener un componente llamado `all-rentals` para mostrar todas las viviendas, y otro componente para mostrar el detalle de una sola vivienda llamado `rental-tile`. El componente `rental-tile` definiría el comportamiento que permite al usuario mostrar u ocultar la propiedad imagen de la vivienda.

Veremos estos conceptos en acción construyendo una aplicación para alquiler de viviendas en la siguiente lección.

## Hooks

En Ember, usamos el término **hook** para métodos que son automáticamente invocados dentro de la aplicación de Ember. Estos métodos son invocados automáticamente, no es necesario invocarlos específicamente.

Algunos ejemplos de un hook son:

* [Hooks del ciclo de vida de un componente](../../components/the-component-lifecycle/): [`willRender()`](https://emberjs.com/api/ember/release/classes/Component/methods/willRender?anchor=willRender/) hook es llamado cada vez antes de renderizar un componente
* Hooks de Rutas: El [`model()`](https://www.emberjs.com/api/ember/release/classes/Route/methods/model?anchor=model/) hook es usado para cargar el modelo en la ruta.

En el siguiente ejemplo, el [`didRender()`](https://emberjs.com/api/ember/release/classes/Component/methods?anchor=didRender/) hook, del ciclo de vida del componente, es usado para registrar el mensaje "¡He renderizado!" en el log de la consola del navegador cada vez que el componente es renderizado.

```javascript {data-filename=/app/components/foo-did-render-example.js}
import Component from '@ember/component';

export default Component.extend({
  didRender() {
    this._super(...arguments);
    console.log('I rendered!');
  }
});
```
