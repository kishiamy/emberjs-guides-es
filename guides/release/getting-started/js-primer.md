Muchas características nuevas fueron introducidas en JavaScript con el lanzamiento de nuevas especificaciones como ECMAScript 2015, también conocido como [ECMAScript 6 o ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla). Mientras que las Guías [asumen que usted tiene un conocimiento práctico de JavaScript](https://guides.emberjs.com/release/#toc_assumptions), no todas las funciones del lenguaje JavaScript pueden ser familiares para el desarrollador.

En esta guía cubriremos algunas características de JavaScript, y cómo se utilizan en aplicaciones de Ember.

## Declaraciones de variables

Una declaración de variables, también llamada vinculante, es cuando se asigna un valor a un nombre de variable. Un ejemplo de declaración de una variable que contiene el número 42 es así:

```javascript
var myNumber = 42;
```

Inicialmente, JavaScript tenía dos formas de declarar variables, globalmente y `var`. Con el lanzamiento de ES2015, `const` y `let` fueron introducidos. Vamos a repasar las diferentes formas de declarar una variable, también llamados bindings porque *vinculan* un valor a un nombre de variable, y por qué el JavaScript moderno tiende a preferir `const` y `let`.

### `var`

Las declaraciones de variables que utilizan `var` existen en todo el cuerpo de la función en la que se declaran. A esto se le llama function-scoping, la existencia del `var` se circunscribe a la función. Si intentas acceder a un `var` fuera de la función se declara, obtendrá un error de que la variable no está definida.

Para nuestro ejemplo, declararemos un `var` llamado `nombre`. Intentaremos acceder tanto dentro como fuera de la función, y ver los resultados que obtenemos:

```javascript
console.log(nombre); // ReferenceError: el nombre no está definido

function myFunction() {
  var = "Tomster";

  console.log(nombre); // "Tomster"
}
```

Esto también significa que si tienes un `if` o un `for` en tu código y declaras un `var` dentro de ellos, todavía puedes acceder a la variable fuera de esos bloques:

```javascript
console.log(nombre); // undefined

if (true) {
  var = "Tomster";

  console.log(nombre); // "Tomster"
}
```

En el ejemplo anterior, podemos ver que el primer `console.log(nombre)` imprime `undefined` en lugar del valor. Esto se debe a una característica de JavaScript llamada *hoisting*. Cualquier declaración de variables es movida por el lenguaje de programación a la parte superior del alcance al que pertenece. Como vimos al principio, `var` se extiende a la función, por lo que el ejemplo anterior es el mismo que el anterior:

```javascript
var nombre;
console.log(nombre); // undefined

if (verdadero) {
  nombre = "Tomster";

  console.log(nombre); // "Tomster"
}
```

#### `const` and `let`

Hay dos grandes diferencias entre `var` y ambos `const` y `let`. `const` y `let` son declaraciones a nivel de bloque, y *no son niveladas*.

Debido a esto, no son accesibles fuera del alcance del bloque dado (es decir, en una `función` o en `{}`) en el que se declaran. Tampoco puede acceder a ellos antes de que sean declarados, u obtendrá un [`ReferenceError`] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError).

```javascript
console.log(nombre) // ReferenceError: el nombre no está definido

if (persona) {
  console.log(nombre) // ReferenceError: el nombre no está definido

  let nombre = 'Gob Bluth'; // "Gob Bluth"
} else {
  console.log(nombre) // ReferenceError: el nombre no está definido
}
```

Las declaraciones `const` tienen una restricción adicional, son *referencias constantes*, siempre se refieren a lo mismo. Para usar una declaración `const` hay que especificar el valor al que se refiere, y no se puede cambiar a qué se refiere la declaración:

```javascript
const nombre; // Uncaught SyntaxError: Falta inicializador en la declaración const
const nombre = 'Gob';
nombre = `George Michael'; // Uncaught SyntaxError: Identificador 'nombre' ya ha sido declarado
```

Tenga en cuenta que `const` no significa que el valor al que se refiere no pueda cambiar. Si tiene un array o un objeto, puede cambiar sus propiedades:

```javascript
const myArray = [];
const myObject = { nombre: "Tom Dale" };

myArray.push(1);
myObject.nombre = "Leah Silber";

console.log(myArray); //[1][1]
console.log(myObject); // {nombre: "Leah Silber"}
```

#### `for` loops

Algo que podría ser confuso es el comportamiento de `let` in `for` loops.

Como vimos antes, las declaraciones `let` se extienden al bloque al que pertenecen. En los bucles `for`, cualquier variable declarada en la sintaxis `for` pertenece al bloque del bucle.

Veamos algún código para ver cómo se ve esto. Si usas `var`, esto sucede:

```javascript
for (var i = 0; i < 3; i++) {
  console.log(i) // 0, 1, 2
}

console.log(i) // 3
```

Pero si usas `let`, esto sucede en su lugar:

```javascript
for (let i = 0; i < 3; i++) {
  console.log(i) // 0, 1, 2
}

console.log(i) // ReferenceError: i no está definido
```

Usar `let` evitará fugas accidentales y cambiará la variable `i` desde fuera del bloque `for`.


## Promesas

Una `Promesa` es un objeto que puede producir un valor en el futuro: un valor resuelto, o una razón por la que no se ha resuelto (por ejemplo, se ha producido un error de red). Una `promesa` puede estar en uno de los 3 estados posibles: `cumplida (fulfilled)`, `rechazada (rejected)`, o `pendiente (pending)`. Las promesas se introdujeron en ES6 JavaScript.

¿Por qué se necesitan promesas en Ember? JavaScript tiene un único subproceso, y algunas cosas como la consulta de datos desde el servidor backend llevan tiempo, bloqueando así el subproceso. Es eficiente no bloquear el hilo mientras se realizan estos cálculos o recuperaciones de datos - ¡Promesas al rescate! Proporcionan una solución devolviendo un objeto proxy por un valor no necesariamente conocido cuando se crea la `Promesa`. Mientras se ejecuta el código `Promise`, el resto del código sigue adelante.

Por ejemplo, declararemos una `Promesa` básica llamada "myPromiseObject".

```javascript
let myPromiseObject = new Promise(function(resolve, reject) {
  // Cuando éxito
  resolve(value);

  // Cuando fracaso
  reject(reason);
});
```

Las promesas vienen equipadas con algunos métodos, de los cuales `then()`y `catch()` son los más comúnmente usados. Puede profundizar en los detalles consultando los enlaces de referencia. `.then()` siempre devuelve una nueva `Promesa`, por lo que es posible encadenar Promesas con un control preciso sobre cómo y dónde se manejan los errores.

Usaremos `myPromiseObject` declarado arriba para mostrarle cómo se usa `then()`:

```javascript
myPromiseObject.then(function(value) { function(value)
  // En el cumplimiento
function(reason) {
  // Cuando rechazo
});
```

Veamos algún código para ver cómo se usan en Ember:

```javascript
store.findRecord('persona', 1).then(function(persona) {
  // Hacer algo con la persona cuando la promesa se resuelve.
  persona.set('nombre', 'Tom Dale');
});
```

En el fragmento anterior, `store.findRecord('persona', 1)` puede hacer una petición de red si los datos no son ya presente en la tienda. Devuelve un objeto `Promise` que puede resolverse casi instantáneamente si los datos están presentes en el almacén, o puede llevar algún tiempo resolver si los datos están siendo recuperados por una petición de red.

Ahora podemos llegar a un punto en el que estas promesas están encadenadas:

```javascript
store.findRecord('persona', 1).then(function(persona) {

  return persona.get('post'); //obtener todos los mensajes relacionados con la persona.

)).then(function(posts){function(posts)

  myFirstPost = posts.get('primerObjeto'); //obtiene el primer mensaje de la colección.
  return myFirstPost.get('comentario'); //obtener todos los comentarios vinculados a myFirstPost.

).then(function(comentarios){función(comentarios){)

  // hacer algo con los comentarios
  return store.findRecord('book', 1); //búsqueda de otro registro

).catch(function(err){

  //Errores de manejo

})
```

En el fragmento de código anterior, asumimos que una persona tiene muchos mensajes, y un mensaje tiene muchos comentarios. Así, `person.get('post')`devolverá un objeto `Promise`. Encadenamos la respuesta con `then()` para que cuando se resuelva, obtengamos el primer objeto de la colección resuelta. Luego, obtenemos comentarios de él con `myFirstPost.get('comment')`que de nuevo devolverá un objeto `Promesa`, continuando así la cadena.

### Recursos

Para más información, puede consultar los artículos de la Red de Desarrolladores:

*[`var`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
*[`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
*[`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let).
*[`promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).
