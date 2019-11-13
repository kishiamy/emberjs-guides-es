## ¿Qué es Ember?

Ember es un marco de trabajo de JavaScript diseñado para ayudarle a construir sitios web con interacciones de usuario ricas y complejas.
Para ello, proporciona a los desarrolladores muchas funciones que son esenciales para gestionar la complejidad de las aplicaciones web modernas,
así como un conjunto de herramientas de desarrollo integrado que permite una rápida iteración.

Algunas de estas características que aprenderá en las guías son:

* [Ember CLI](../../configurando-ember/configurando-ember-cli/) - Un robusto kit de herramientas de desarrollo para crear, desarrollar y construir aplicaciones Ember. Cuando veas una instrucción `$ ember <command>` a través de las guías, eso es Ember CLI!
* [Routing](../../routing/) - La parte central de una aplicación Ember. Permite a los desarrolladores controlar el estado de la aplicación desde la URL.
* [Templating engine](../../templates/handlebars-basics/) - Use la sintaxis del manillar para escribir las plantillas de su aplicación.
* [Capa de datos](../../modelos/) - Ember Data proporciona una manera consistente de comunicarse con APIs externas y administrar el estado de la aplicación.
* [Ember Inspector](../../ember-inspector/) - Una extensión del navegador, o bookmarklet, para inspeccionar su aplicación en vivo. También es útil para detectar aplicaciones Ember en la naturaleza, intente instalarlo y abra el [sitio web de la NASA](https://www.nasa.gov/)!

## Organización

En el lado izquierdo de cada página de las Guías hay una tabla de contenidos,
organizado en secciones que pueden ser ampliadas para mostrar los temas
que ellos cubren. Tanto las secciones como los temas dentro de cada sección son
ordenados de conceptos básicos a avanzados.

Las Guías pretenden contener explicaciones prácticas sobre cómo
construir aplicaciones Ember, centrándose en las características más utilizadas de Ember.js.
Para una documentación completa de cada característica y API de Ember, consulte la sección
[Documentación de la API de Ember.js] (https://api.emberjs.com/).

Los Guías comienzan con una explicación de cómo comenzar con Ember,
seguido de un tutorial sobre cómo construir tu primera aplicación Ember.
Si eres nuevo en Ember, le recomendamos que empiece por seguir estas dos primeras secciones de las Guías.

## Supuestos

Mientras tratamos de hacer que las Guías sean lo más amigables posible para los principiantes,
debemos establecer una línea de base para que las guías puedan mantenerse enfocadas en la funcionalidad de Ember.js.
Trataremos de enlazar con la documentación apropiada cada vez que se introduzca un concepto.

Para sacar el máximo provecho de las guías, usted debe tener un conocimiento práctico de:

* **HTML, CSS, JavaScript** - los bloques de construcción de páginas web. Puede encontrar documentación de cada una de estas tecnologías en[Mozilla Developer Network] (https://developer.mozilla.org/en-US/docs/Web).
* **Promesas** - la forma nativa de tratar la asincronía en su código JavaScript. Consulte la sección correspondiente[Mozilla Developer Network] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).
* **Módulos JavaScript** - comprenderá mejor la estructura del proyecto y las rutas de importación de[Ember CLI's](https://ember-cli.com/) si se siente cómodo con[Módulos JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules).
* **Sintaxis moderna** - Ember CLI viene con Babel.js por defecto para que puedas
aprovechar las nuevas funciones del lenguaje, como las funciones Flecha, plantillas de cadena, desestructuración y más. Puede verificar la
[Documentación de Babel.js](https://babeljs.io/docs/learn-es2015/) o lea [Entendiendo ECMAScript 6](https://leanpub.com/understandinges6/read) en línea.

## Una nota sobre el rendimiento móvil

Ember le ayudará a escribir aplicaciones web más rápido, pero no puede evitar que escriba una aplicación lenta. Esto es especialmente cierto en los dispositivos móviles. Para ofrecer una gran
experiencia, es importante medir el rendimiento constantemente y con un conjunto diverso de dispositivos.

Asegúrese de que está probando el rendimiento en dispositivos reales. Los simuladores móviles
en un ordenador personal ofrecen una representación optimista en comparación con el rendimiento 
que tendrá en el mundo real. Mientras más sistemas operativos y
configuraciones de hardware sean probados, más seguro puede estar.

Debido a su limitada conectividad de red y potencia de CPU, es raro obtener gran rendimiento en
los dispositivos móviles gratuitamente. Debería integrar las pruebas de rendimiento
en su flujo de trabajo de desarrollo desde el principio. Esto le ayudará a evitar
costosos errores de arquitectura de software que son más difíciles de arreglar si 
se detectan cuando su aplicación esté casi completa.

En resumen:

1. Pruebe siempre en dispositivos móviles reales y representativos.
2. Mida el rendimiento desde el principio y siga probando conforme sigue desarrollando su aplicación.

Estos consejos le ayudarán a identificar los problemas a tiempo para que puedan ser abordados sistemáticamente, en lugar de
un doloroso rediseño de último minuto.

## Informar de un problema

Typos(errores tipográficos), las palabras faltantes y los ejemplos de código con errores son considerados 
errores de documentación. Si descubre uno de ellos, o quiere mejorar las guías existentes
de alguna manera, ¡estaremos encantados de ayudarle a ayudarnos!

Algunas de las formas más comunes de reportar un problema con las guías son:

* Usando el ícono del lápiz en la parte superior derecha de cada página de la guía
* Abrir una incidencia o solicitud pull a [el repositorio de GitHub] (https://github.com/ember-learn/guides-source/)

Haciendo clic en el icono del lápiz le llevará al editor de GitHub de la guía seleccionada para modificarla inmediatamente, usando el lenguaje de marcado de texto Markdown.
Esta es la manera más rápida de corregir un error tipográfico, una palabra faltante o un error de una muestra de código.

Si desea hacer una contribución más significativa, no dude en consultar nuestro
[Seguimiento de Incidencias](https://github.com/ember-learn/guides-source/issues) para ver si su problema ya está siendo atendido. Si no encuentra un problema activo, abra uno nuevo.

Si tiene alguna pregunta sobre el estilo o el proceso de contribución, usted puede consultar
  nuestra [guía para contribuir] (https://github.com/ember-learn/guides-source/blob/master/CONTRIBUTING.md). Si su pregunta persiste, comuníquese con nosotros a través del canal `#dev-ember-learning` en [Ember Community Discord](https://discordapp.com/invite/zT3asNS).

¡Buena suerte!
