¡Bienvenido al tutorial de Ember! Este tutorial está destinado para introducir conceptos básicos de Ember al crear una aplicación de aspecto profesional. Si se queda atascado en cualquier momento durante el tutorial, siéntase libre de descargar [https://github.com/ember-learn/super-rentals](https://github.com/ember-learn/super-rentals) para un ejemplo de trabajo de la aplicación completa.

Puede instalar la última versión de `Ember CLI` siguiendo el Guía de [Quick Start](../getting-started/quick-start/#toc_install-ember) Sección "Instalando Ember".

Ember CLI, la interfaz de línea de comando de Ember, proporciona un proyecto de estructura estándar, un conjunto de herramientas de desarrollo, y un sistema de addons. Esto permite a los desarrolladores de Ember centrarse en la construcción de aplicaciones en lugar de que construir las estructuras de soporte que las hacen funcionar. Desde su línea de comandos, un rápido `ember --help` ayuda muestra los comandos que Ember CLI proporciona. Para más información sobre un comando específico, escriba `ember help <nombre del comando>`.

## Creando una nueva aplicación

Para crear un nuevo proyecto usando Ember CLI, usa el comando `new`. En la preparación para el tutorial de la siguiente sección, puede hacer una aplicación llamada `super-rentals`.

```bash
ember new super-rentals
```

Se creará un nuevo proyecto dentro de su directorio actual. Ahora puede ir a su directorio de proyectos de `super-rentals` y empezar a trabajar en él.

```bash
cd super-rentals
```

## Estructura del directorio

El comando `new` genera una estructura de proyecto con los siguientes archivos y directorios:

```text
|--app
|--config
|--node_modules
|--public
|--tests
|--vendor

<other files>

ember-cli-build.js
package.json
README.md
testem.js
```

Echemos un vistazo a las carpetas y archivos que genera Ember CLI.

**app**: Aquí es donde las carpetas y archivos para los modelos, componentes, rutas, se almacenan plantillas y estilos. La mayor parte de su codificación del proyecto en Ember ocurre en esta carpeta.

**config**: El directorio config contiene el `environment.js` donde puede configurar los ajustes de su aplicación.

**node\_modules / package.json**: Este directorio y archivo son de npm. npm es el administrador de paquetes de Node.js. Ember está construido con Node y usa variedad de módulos Node.js para operar. El archivo `package.json` mantiene la lista de dependencias npm actuales para la aplicación. Cualquier complemento de Ember CLI que instale también aparecerá aquí. Los paquetes listados en `package.json` están instalados en el directorio node\ _modules.

**public**: Este directorio contiene tanto imágenes como fuentes.

**vendor**: Este directorio es donde van las dependencias del front-end (como JavaScript o CSS) que no son manejados por npm.

**tests / testem.js**: Las pruebas automatizadas de nuestra aplicación van en la carpeta `tests`, y el ejecutador de pruebas de Ember CLI **Testem** está configurado en `testem.js`.

**ember-cli-build.js**: Este archivo describe cómo Ember CLI debe construir nuestra aplicación.

## Módulos

Si miras en el `app/router.js`, notarás una sintaxis que puede que no sea familiar.

```javascript {data-filename=app/router.js}
import EmberRouter from '@ember/routing/router';
import config from './config/environment';

const Router = EmberRouter.extend({
  location: config.locationType,
  rootURL: config.rootURL
});

Router.map(function() {
});

export default Router;
```

Ember CLI utiliza módulos de JavaScript para organizar el código de la aplicación.
Por ejemplo, la línea `import EmberRouter de "@ember/routing/router";` nos da acceso a la clase de Ember Router como la variable `EmberRouter`. Y la línea `import config from './config/environment';` nos da acceso a los datos de configuración de nuestra aplicación como la variable `config`.  `Const` es una forma de declarar una variable de sólo lectura para asegurarse de que no se reasigna accidentalmente a otro lugar. Al final del fichero, `export default Router;` hace que la variable `Router` definida en este fichero esté disponible para otras partes de la aplicación.


## El servidor de desarrollo

Una vez que tengamos un nuevo proyecto, podemos confirmar que todo funciona iniciando el servidor de desarrollo Ember:

```bash
ember serve
```

or, for short:

```bash
ember s
```

Si navegamos a [`http://localhost:4200`](http://localhost:4200), veremos la pantalla de bienvenida por defecto.

![pantalla de bienvenida por defecto](/public/images/tutorial/default-welcome-page.png)

Lo primero que queremos hacer en nuestro nuevo proyecto es quitar la pantalla de bienvenida. Lo hacemos editando el archivo de la plantilla de la aplicación que se encuentra en `app/templates/application.hbs`. Como su nombre indica, esta plantilla es para la aplicación en sí, y los cambios en ella se reflejarán a lo largo de toda la aplicación. Esta plantilla se utiliza más a menudo para albergar el diseño de la aplicación, normalmente contiene el encabezado, el pie de página, la barra de navegación, etc.

Cuando editemos el archivo `app/templates/application.hbs`, eliminaremos el componente etiquetado como `<WelcomePage />`:

```handlebars {data-filename="app/templates/application.hbs" data-diff="-1,-2,-3"}
{{!-- The following component displays Ember's default welcome message. --}}
<WelcomePage />
{{!-- Feel free to remove this! --}}

{{outlet}}

```

La aplicación ahora debería ser un lienzo completamente en blanco para construir nuestra aplicación.
