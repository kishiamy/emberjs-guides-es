Muchas características nuevas fueron introducidas en JavaScript con el lanzamiento de nuevas especificaciones como ECMAScript 2015,
también conocido como[ECMAScript 6 o ES6] (https://developer.mozilla.org/en/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla).
Mientras que las Guías[asumen que usted tiene un conocimiento práctico de JavaScript](/#toc_asuntos),
no todas las funciones del lenguaje JavaScript pueden ser familiares para el desarrollador.

En esta guía cubriremos algunas características de JavaScript,
y cómo se utilizan en aplicaciones de Ember.

## Declaraciones de variables

Una declaración de variables, también llamada vinculante, es cuando se asigna un valor a un nombre de variable.
Un ejemplo de declaración de una variable que contiene el número 42 es así:

```javascript
var myNumber = 42;
```

Inicialmente, JavaScript tenía dos formas de declarar variables, globalmente y `var`.
Con el lanzamiento de ES2015, `const` y `let` fueron introducidos.
Vamos a repasar las diferentes formas de declarar una variable,
también llamados bindings porque *vinculan* un valor a un nombre de variable,
y por qué el JavaScript moderno tiende a preferir `const` y `let`.

### `var`

Las declaraciones de variables que utilizan `var` existen en todo el cuerpo de la función en la que se declaran.
A esto se le llama function-scoping, la existencia del `var` se circunscribe a la función.
Si intentas acceder a un `var` fuera de la función se declara,
obtendrá un error de que la variable no está definida.

Para nuestro ejemplo, declararemos un `var` llamado `nombre`.
Intentaremos acceder tanto dentro como fuera de la función,
y ver los resultados que obtenemos:

```javascript
console.log(name); // ReferenceError: el nombre no está definido

function myFunction() {
  var = "Tomster";

  console.log(name); // "Tomster"
}
```

Esto también significa que si tienes un "si" o un "para" en tu código y declaras un "var" dentro de ellos,
todavía puedes acceder a la variable fuera de esos bloques:

```javascript
console.log(name); // undefined

si (verdadero) {
  var = "Tomster";

  console.log(name); // "Tomster"
}
```

En el ejemplo anterior, podemos ver que el primer `console.log(nombre)` imprime `indefinido` en lugar del valor.
Esto se debe a una característica de JavaScript llamada *hoisting*.
Cualquier declaración de variables es movida por el lenguaje de programación a la parte superior del alcance al que pertenece.
Como vimos al principio, `var` se extiende a la función,
por lo que el ejemplo anterior es el mismo que el anterior:

```javascript
var nombre;
console.log(name); // undefined

si (verdadero) {
  nombre = "Tomster";

  console.log(name); // "Tomster"
}
```

#### `const` and `let`

Hay dos grandes diferencias entre `var` y ambos `const` y `let`.
const` y `let` son declaraciones a nivel de bloque, y no son *niveladas*.

Debido a esto, no son accesibles fuera del alcance del bloque dado (es decir, en una `función` o en `{}`) en el que se declaran.
Tampoco puede acceder a ellos antes de que sean declarados, o obtendrá un[`ReferenceError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError).

```javascript
console.log(name) // ReferenceError: el nombre no está definido

si (persona) {
  console.log(name) // ReferenceError: el nombre no está definido

  let name = 'Gob Bluth'; // "Gob Bluth"
} más {
  console.log(name) // ReferenceError: el nombre no está definido
}
```

Las declaraciones `const` tienen una restricción adicional, son *referencias constantes*,
siempre se refieren a lo mismo.
Para usar una declaración `const` hay que especificar el valor al que se refiere,
y no se puede cambiar a qué se refiere la declaración:

```javascript
const firstName; // Uncaught SyntaxError: Falta inicializador en la declaración const
const firstName = 'Gob';
firstName = `George Michael'; // Uncaught SyntaxError: Identificador 'firstName' ya ha sido declarado
```

Tenga en cuenta que `const` no significa que el valor al que se refiere no pueda cambiar.
Si tiene un array o un objeto, puede cambiar sus propiedades:

```javascript
const myArray = [];
const myObject = { nombre: "Tom Dale" };

myArray.push(1);
myObject.name = "Leah Silber";

console.log(myArray); //[1][1]
console.log(myObject); // {nombre: "Leah Silber"}
```

#### `for` loops

Algo que podría ser confuso es el comportamiento de ``let` in `for` loops.

Como vimos antes, las declaraciones "vamos" se extienden al bloque al que pertenecen.
En los bucles `for`, cualquier variable declarada en la sintaxis `for` pertenece al bloque del bucle.

Veamos algún código para ver cómo se ve esto.
Si usas `var`, esto sucede:

```javascript
para (var i = 0; i < 3; i++) {
  console.log(i) // 0, 1, 2
}

console.log(i) // 3
```

Pero si usas "vamos", esto sucede en su lugar:

```javascript
para (deje i = 0; i < 3; i++) {
  console.log(i) // 0, 1, 2
}

console.log(i) // ReferenceError: i no está definido
```

Usar `let` evitará fugas accidentales y cambiará la variable `i` desde fuera del bloque `for`.

## Promesas

Una `Promesa' es un objeto que puede producir un valor en el futuro: un valor resuelto, o una razón por la que no está resuelto (por ejemplo, una red).
