Comenzar a usar Ember es fácil. Los proyectos de Ember son creados y administrados por línea de comandos vía la herramienta Ember CLI.
Las ventajas que nos ofrece esta herramienta son:

* Ofrece tareas modernas para el tratamiento de assets como: concatenación, minificación, versionamiento y demás manejos de archivos.
* Generadores para crear componentes, rutas y más.
* Una estructura convencional de proyectos que hace fácil la adopción de Ember.
* Soporte para Javascript moderno via [Babel](https://babeljs.io/). Esto incluye soporte para [Módulos JavaScript](http://exploringjs.com/es6/ch_modules.html), que son usados a lo largo esta guía.
* Una completa guía de pruebas con [QUnit](https://qunitjs.com/).
* La habilidad de consumir un creciente ecosistema de [Ember Addons](https://emberobserver.com/).

## Dependencias

### Git

Ember requiere Git para controlar muchas de sus dependencias. Git viene incluído con Mac OS
X y la mayoría de distribuciones de Linux. Usuarios de Windows puede descargar y ejecutar [este instalador de Git](http://git-scm.com/download/win).

### Node.js y npm

Ember CLI es construido con JavaScript, y requiere la más reciente versión LTS del runtime de [Node.js](https://nodejs.org/).
También requiere dependencias descargadas vía [npm](https://www.npmjs.com/). npm es empacado con Node.js, así que si tu computadora tiene Node.js
instalado entonces estás listo para comenzar.

Si no está seguro de cuál es la versión de Node.js o la versión correcta para usar, puede ejecutar este comando en su terminal:

```bash
node --version
npm --version
```

Si la ejecución retorna un error de tipo *"command not found"* o una versión antigua de Node:

* Usuarios Windows o Mac pueden descargar y ejecutar [este instalador de Node.js](http://nodejs.org/en/download/).
* Usuarios Mac generalmente prefieren instalar Node usando [Homebrew](http://brew.sh/). Después de
instalar Homebrew, ejecute `brew install node` para instalar Node.js. Alternativamente, el instalador de paquetes están disponible directamente 
desde [Node.js](https://nodejs.org/en/download/).
