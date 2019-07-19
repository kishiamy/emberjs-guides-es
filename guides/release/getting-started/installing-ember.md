Comenzar con Ember es fácil. Los proyectos de Ember se crean y administran a través de nuestra herramienta de creación de línea de comandos, la CLI de Ember.
Esta herramienta proporciona:

* Moderna gestión de activos de aplicaciones (incluyendo concatenación, minificación y control de versiones).
* Generadores para ayudar a crear componentes, rutas y más.
* Un diseño de proyecto convencional, que hace que las aplicaciones Ember existentes sean fáciles de abordar.
* Soporte para ES2015 / ES6 JavaScript a través del proyecto [Babel] (https://babeljs.io/learn-es2015/). Esto incluye soporte para [módulos JavaScript] (http://exploringjs.com/es6/ch_modules.html), que se utilizan en esta guía.
* Un set de prueba completo de [QUnit] (https://qunitjs.com/).
* La capacidad de consumir un creciente ecosistema de [Ember Addons] (https://emberobserver.com/).

## Dependencias

### Git

Ember requiere que Git gestione muchas de sus dependencias. Git viene con Mac OS X y la mayoría de las distribuciones de Linux. Los usuarios de Windows pueden descargar y ejecutar [este instalador de Git (http://gitscm.com/download/win).

### Node.js y npm

La CLI de Ember está construida con JavaScript y requiere la versión LTS más reciente del tiempo de ejecución [Node.js] (https://nodejs.org/). También requiere dependencias obtenidas a través de [npm] (https://www.npmjs.com/). npm está empaquetado con Node.js, por lo que si su computadora tiene Node.js instalado, está listo para comenzar.

Si no está seguro de si tiene Node.js o la versión correcta, ejecute esto en su línea de comando:

```bash
node --version
npm --version
```

Si aparece un error * "comando no encontrado" * o una versión desactualizada del paquete:

* Los usuarios de Windows o Mac pueden descargar y ejecutar [este instalador Node.js] (http://nodejs.org/en/download/).
* Los usuarios de Mac a menudo prefieren instalar Node usando [Homebrew] (http://brew.sh/). Después de instalar Homebrew, ejecute `brew install node` para instalar Node.js. Alternativamente, los paquetes de instalación están disponibles directamente desde [Node.js] (https://nodejs.org/en/download/).
* Los usuarios de Linux pueden usar [esta guía para la instalación de Node.js en Linux (https://nodejs.org/en/download/package-manager/).

Si obtiene una versión obsoleta de npm, ejecute `npm install -g npm`.

### Watchman (opcional)

En Mac y Linux, puede mejorar el rendimiento de la visualización de archivos instalando [Watchman (https://facebook.github.io/watchman/docs/install.html).

## Instalación

Instala Ember usando npm:

```bash
npm install -g ember-cli
```

Para verificar que su instalación fue exitosa, ejecute:

```bash
ember -v
```

Si se muestra un número de versión, estás listo para continuar.
