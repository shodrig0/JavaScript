# Teoría de JS

El fin de realizar este trabajo práctico de JS, es para poder afianzar los conceptos teóricos del lenguaje para entender su funcionamiento, como también poder practicar un poco lo básico e invitar a quienes estén interesados en aprender.

## ¿Qué significan los conceptos de scope, hoisting y mutabilidad en JavaScript?

Estos son tres conceptos básicos necesarios para aprender cómo funciona este lenguaje de programación, ya que nos describen el compilado y la ejecución del entorno.

- Scope: El alcance en el que una variable puede ser 'visible' o referenciada. En JS, una variable por fuera de una función, está en un alcance global para todas las funciones. Por el contrario, si la variable está declarada dentro de una función, no podrá ser invocada afuera o en otra función, ya que se considerarían contextos diferentes.

```js
// Alcance global
let x = 'Hola mundo!' // --> esta es una variable global

const saludo = () => {
    console.log(x)
}

saludo() // --> la salida es 'Hola mundo!', tal cual valor de x
console.log(x) // --> también veremos un 'Hola mundo!'

// Alcance local dentro de una función
const despedida = () => {
    let y = 'Chau mundo!!'
    console.log(y)
}

despedida() // --> la salida será 'Chau mundo!!' tal cual el valor de z
console.log(y) // --> error porque z no está definida en el ámbito global
```
- Hoisting: Dicho de una forma más amigable, el hoisting es la declaración de las variables a la hora de compilar el código de JS. En este proceso, todas las variables que son declaradas se toman como 'fueron escritas al principio del todo' (se elevan), aunque estén escritas en diferentes partes del código.

```js
const saludoCustom = (nombre) => {
    console.log(`Hola ${nombre}! Cómo estás?`)
}

saludoCustom('Rodrigo') // --> la salida de esta función será "Hola Rodrigo, como estás?"

// En este caso más normal, por más que pasemos el parámetro después (en la invocación), al compilar el código lo que ocurre es algo así:

saludoCustom('Rodrigo') // --> se guarda en memoria la declaración

function saludoCustom(nombre) { // --> una arrow function es lo mismo que escribir el código de esta manera clásica
    console.log(`Hola ${nombre}! Cómo estás?`)
}

// De nuevo, el resultado no varía. Pero qué pasa si no obtenemos el resultado esperado?

let x = 1

console.log(x + y) // --> tecnicamente estamos haciendo 1 + undefined, por lo que dará error

let y = 2

// Pero por qué? Técnicamente la compilación entiende el código así:
let x = 1
let y
console.log(x + y) // --> la variable 'y' está declarada antes de llamar al console.log, por lo que el resultado será un error como un Not a Number (NaN)
y = 2

// Lo correcto sería:
let x = 1
let y = 2
console.log(x + y) // --> 3
```
- Inmutabilidad y Mutabilidad: Para entender mejor estos conceptos en JS debemos entender que existen dos tipos de datos: primitivos y no primitivos.
    * Primitivos: strings, number, boolean, null, undefined, bigint y Symbol.
    * No Primitivos: objetos y arrays.
```js
// Los tipos primitivos, una vez que se les asigna un valor a la variable, no será posible cambiarselo asignandole otro valor.
let variable = 'Hola Mundo'
console.log(variable) // --> la salida será 'Hola Mundo'
variable  = 'Chau Mundo!' // --> SI DEJÁRAMOS ASÍ lo que está pasando es que terminaríamos asignando otro espacio en memoria. Por ende, lo que decía en primer instancia, será borrado para poder reescribirse la segunda parte. En otras palabras, se está creando una nueva instancia a la fuerza
console.log(variable) // --> la salida será 'Chau Mundo!'
```
Pero si quisiéramos mantener ese espacio en memoria:
```js
let variable = 'Hola Mundo'
console.log(variable) // --> la salida será 'Hola Mundo'
variable[0] = 'Chau' // --> 0 hace referencia a la palabra 'Hola', la primera que encontraría en ese ese 'array'
console.log(variable) // --> la salida seguirá siendo 'Hola Mundo'
```
En otras palabras, la variable tipo _string_ mantuvo su valor y espacio en memoria no se vio afectado, por lo que inferimos que los datos **Primitivos** son **Inmutables**. Si quisiéramos reemplazar _Hola_ a _Chau_, lo que deberíamos hacer es utilizar la función **replace()** de JS y crear una nueva instancia:
```js
let variable1 = 'Hola Mundo'
let variable2 = variable1.replace('Hola','Chau')
console.log(variable1) // --> 'Hola Mundo'
console.log(variable2) // --> 'Chau Mundo'
```
Ahora dando por sentado que los datos **No Primitivos** son **Mutables**, tenemos que considerar que acá si se puede alterar el estado y las propiedades de un objeto/array en JS, sin tener que crear una nueva instancia ni forzando el reemplazo total en memoria.
```js
const persona = {
    nombre: 'Rodrigo',
    edad: 27
}

console.log(persona) // --> { nombre: 'Rodrigo', edad: 27 }

persona.estado = 'Estudiante' // --> al obj, que en su primer instancia sólo tiene nombre y edad, podemos agregarle una nueva característica

console.log(persona) // --> { nombre: 'Rodrigo', edad: 27, estado: 'Estudiante' }
```
Por lo que cuando hablamos de que un dato mutable en JS (y en otros lenguajes) estamos diciendo que tiene la capacidad de cambiar su estado, ya sea cambiando el valor de la propiedad de un objeto o la referencia que tiene una variable. Por ejemplo, los **objetos** son de tipo referencia, por lo que a una variable a la que se le asigna un objeto, en realidad se le está definiendo una _referencia_ a ese objeto. Por eso, si a otra variable se le asigna una referencia al mismo objeto y lo modifica, esto afectará a todas las variables vinculadas.
```js
const persona = {
    nombre: 'Rodrigo',
    edad: 27
}
console.log(persona) // --> { nombre: 'Rodrigo', edad: 27 }

const otraPersona = persona
otraPersona.nombre = 'Xavier'

console.log(persona) // --> { nombre: 'Xavier', edad: 27 }
```
## ¿Qué diferencias hay entre var, let y const en cuanto a scope, hoisting y mutabilidad? ¿Cuándo usarías uno y cuándo otro?

Ya sabemos que las tres formas son para declarar una variable. En un principio se utilizaba **var** para todo. Tanto para el ámbito global como el local, o sea, fuera y dentro de funciones
```js
// Recordemos el principio de scope
var saludo = 'Hola mundo'

function saludar() {
    var saludo = 'Hooola Mundoooo' // Si recordamos que en el scope de una función, si la variable de acá adentro se llamara 'otroSaludo', si ponemos console.log(otroSaludo) dará el error de undefined
}

console.log(saludo) // --> 'Hola Mundo'
console.log(saludar()) // --> undefined, porque la función sigue sin retornar algo.
```
Pero el problema de **var** es que se puede reasignar un nuevo valor a una variable ya declarada.
```js
var saludar = 'Hello there!'
console.log(saludar) // --> 'Hello there!'
var saludar = 1 // --> o directamente poner: saludar = 1
console.log(saludar) // --> 1 (sí, también se puede cambiar el tipo de dato)
```
Por eso, si ya usaste una variable antes en tu código y por algún motivo la volves a utilizar, lo más seguro es que obtengas una salida muy diferente a la que esperabas
```js
var saludo = 'Hola Mundo'
var diaLindo = false

if (!diaLindo) {
    var saludo = 'Hola Mundo triste'
}

console.log(saludo) // --> 'Hola Mundo triste'... por lo que el valor fue pisado
```
Para evitar esta clase de errores, es donde surgen **let** y **const**. Si vemos al primero, su funcionalidad tiene un ámbito en bloque. Un bloque es una porción de código delimitado por { }, o sea un bloque está entre llaves y todo lo que está adentro es parte del bloque. Por lo que una variable declarada en un bloque con **let** solo está disponible para su uso dentro de ese bloque.
```js
let saludo = 'Hola Mundo'
let diaLindo = false

if (!diaLindo) {
    let otroSaludo = 'Hola mundo triste'
    console.log(otroSaludo) // --> 'Hola mundo triste'
}

console.log(otroSaludo) // --> No existe ese variable fuera
```
Y qué pasa si declaramos la variable _saludo_ en diferentes bloques?
```js
let saludo = 'Hola Mundo'
let diaLindo = false

if (!diaLindo) {
    let saludo = 'Hola mundo triste'
    console.log(saludo) // --> 'Hola mundo triste'
}

console.log(saludo) // --> 'Hola mundo'
```
Eso ocurre porque el que está dentro de la condición _if (!diaLindo)_ se considera una variable completamente distinta, o sea que está asignada en otra parte de la memoria. También no está de más aclarar que una variable declarada con **let** puede volver a tener un valor distinto, pero no volver a declarse. Para que se entienda, puede ocurrir esto:
```js
let saludar = 'Hola mundo!'
saludar = 'Hello world!'
console.log(saludar) // --> 'Hello world!'
```
Pero no esto:
```js
let saludar = 'Hola mundo!'
let saludar = 'Hello world!' // --> Se produce un error sobre el bloque de scope que no permite que el código compile
console.log(saludar)
```
Y por último queda **const**, que también poseen un ámbito/contexto/scope de bloque, al igual que las variables declaradas con **let**. En este caso, las variables declaradas con **const** mantendrán un valor constante, lo que no permitirá reasignarle un valor como más arriba ocurrió con **let**:
```js
let saludar = 'Hola mundo!'
saludar = 'Hello world!' // --> Error
console.log(saludar)
```
Las variables deben contener el valor una vez que se vayan a declarar:
```js
const saludar // --> Error
saludar = 'Hola mundo'
```
Tampoco permite declarar dos variables con el mismo nombre, aunque posean datos distintos.
```js
const saludar = 'Hola mundo!'
const saludar = 'Hello world!' // --> Se produce el mismo error que con let
console.log(saludar)
```
Aunque con **const** los objetos funcionan de manera diferente:
```js
const saludar = {
    mensaje: "Hola mundo",
    diaLindo: false
}
saludar.mensaje = "Hello world"

console.log(saludar) // --> { mensaje: 'Hello world', diaLindo: false }
```
Pero no podemos hacer esto:
```js
const saludar = {
    mensaje: "Hola mundo",
    diaLindo: false
}

saludar = {
    texto: 'Acá hay otro saludo!',
    estadoDelDia: true
} // --> Error!
```
Por lo tanto, qué utilizaría uno y cuándo otro?
- **var**: Sí y sólo sí tendré una variable que no estará en otra parte del código y quiero utilizarla dentro de una función. También, para crear mutables.
- **let**: Al igual que con **var**, para poder crear variables mutables y definirlas en el scope del bloque correspondiente, evitando así errores.
- **const**: Si necesito que una variable sea inmutable, permitiéndome así utilizarlas incluso para declarar una función del tipo flecha.

## ¿Qué pasa si intentás usar una variable declarada con **let** antes de su declaración? ¿Y con **var**?

Si intentara compilar este código:
```js
saludo = 'Hola'
let saludo

console.log(saludo) // --> Error!
```
El error que se produce es porque no se puede acceder al valor de una variable que todavía no está declarada con **let**. Pero curiosamente, con **var** si funciona:
```js
saludo = 'Hola'
var saludo

console.log(saludo) // --> 'Hola'
```
Y esto se debe a que con **let**, esa variable (si bien en un console.log tendrá un valor _undefined_) todavía no está asignada en la memoria. En otras palabras, hasta que no llegue a **let** no habrá leído esa porción de código en bloque, por lo que no puede leer un valor antes de ser declarado. A diferencia de **var** que ignora esa porción y considera todo un bloque global.

## ¿Qué significa que JavaScript sea un lenguaje de tipado dinámico?

Que JS sea un lenguaje de tipado dinámico significa que en el momento de ejecución, a las variables se les asigna un _tipo_ basado en el _valor_ del momento.
```js
let saludo = 'Hola!'
let diaLindo = false

console.log(typeof (saludo)) // --> string

if (!diaLindo) {
    saludo = 110105114114117 // 'Hola' en base octal
    console.log(typeof (saludo)) // --> number
}
console.log(typeof (saludo)) // --> number (perdura el último tipo de dato asignado)
```

## ¿Cuál es la diferencia entre undefined y null en JavaScript?

Primero tenemos que entender qué significa cada uno y luego compararemos:

- **undefined**: Indica que una variable fue declarada pero no se le asignó ningún valor.
```js
let saludo
console.log(saludo) // --> undefined

var nombre
console.log(nombre) // --> undefined

// Recordar que const no permite una declaración sin inicialización, pero podemos hacer lo siguiente
const animal = undefined // --> Podemos declarar intencionalmente una variable que tendrá un estado indefinido, pero es sólo en situaciones muy especiales
console.log(animal) // --> undefined
```
- **null**: Significa que a una variable se le asignó el valor de **vacío** directamente. Esa es la principal diferencia con la anterior manera:
```js
let saludo = null
console.log(saludo) // --> null

var nombre = null
console.log(nombre) // --> null

const animal = null
console.log(animal) // --> null
```
Entendemos que si está null, es porque el desarrollador decidió que así fuera. Por ejemplo:
```js
let saludo = 'Hola Mundo'
let diaLindo = false

if (!diaLindo) {
    saludo = null
    console.log(saludo) // --> null, podemos decir que no hay saludo si el día está feo
    if (saludo == null) console.log('El mundo apesta') // --> Y si no hay saludo, decimos que el mundo apesta
} else {
    console.log(saludo)
}
```
## ¿Qué tipo de valor es NaN y en qué situaciones puede aparecer?

**NaN** significa Not-A-Number, y es una propiedad que es utilizada cuando el resultado de una operación no es un número válido.
```js
let num1 = 'A'
let num2 = null

console.log(num1 / num2) // --> NaN

let n1 = 'A'
let n2 = 4

const suma = (a, b) => {
    // parseamos los valores de los parámetros ingresados como enteros
    let numero1 = parseInt(a)
    let numero2 = parseInt(b)
    return numero1 + numero2 // suma un valor no numérico + 4
}

console.log(suma(n1, n2)) // --> NaN
```
Para definir, NaN es una propiedad del objeto global y del objeto _Number_, es del tipo numérico pero no se define como un número real como tal, por lo que nunca es equivalente a otro número. También podemos usar la función _isNan()_ para validar si el valor es un NaN:
```js
let n1 = 'A'
let n2 = 4

const suma = (a, b) => {
    let numero1 = parseInt(a)
    let numero2 = parseInt(b)
    let resultado = numero1 + numero2
    if (isNaN(resultado)) { // --> validamos acá
        return 'El resultado no es numérico' // --> retornamos el mensaje acá
    }
    return resultado
}

console.log(suma(n1, n2))
```

## ¿Qué hace el operador _typeof_ y qué valores puede devolver?

Lo que hace _typeof()_ es devolver una cadena que indica el tipo del operando. Por ejemplo:
```js
let saludo = 'Hola Mundo'
console.log(typeof (saludo)) // --> string

let numero = 25
console.log(typeof (numero)) // --> number

let esVerdad = true
console.log(typeof (esVerdad)) // --> boolean

let noExiste
console.log(typeof (vacio)) // --> undefined

let objeto = {
    nombre: 'Auto',
    color: 'Rojo'
}
console.log(typeof (objeto)) // --> object

let coleccion = []
console.log(typeof (coleccion)) // --> object

let vacio = null
console.log(typeof (vacio)) // --> object
```

## ¿Por qué typeof null devuelve "object" y por qué se considera un bug histórico?

Como se ve arriba, en los casos de **array** y **null**, el operador _typeof()_ no retorna _array_ y _null_ respectivamente, sino que los trata como objetos. Por qué?
- Para el caso de **null** porque por lo general se usa en objetos, para indicar una referencia vacía a un objeto.
- Y los **arrays**, básicamente son un tipo especial de objetos.

Se considera un bug histórico porque al creador de JS se le hizo cómodo referenciar de esa forma el tipo de nulo.

## ¿Qué diferencias hay entre usar comillas simples, dobles o backticks para strings?

El uso de comillas en JS, como en otros lenguajes, puede tener una sutil diferencia a la hora de escribir. Aunque el uso entre simples y dobles no puede tener tanta diferencia, sí hay que considerar que con las comillas dobles podemos hacer un salto de línea utilizando _'\n'_.
```js
console.log("Hola\nMundo!") // --> salto de línea

// Pero luego no vemos diferencia entre este código y este otro
const saludar = (nombre) => {
    console.log('Hola' + " " + nombre) // --> la salida seguirá siendo >> Hola Rodrigo <<
}

saludar("Rodrigo")
```
Lo cierto que entre las comillas simples y dobles lo que en realidad importa es el convenio/formateo que contenga el proyecto. Si se decide utilizar solo comillas simples, lo ideal es utilizar sólo esas. Lo mismo si es en el caso de comillas dobles.

Ahora el caso de los **backticks** (estamos hablando de estas: ``` `` ```) se utilizan para incluir las variables directamente en un retorno como un mensaje o tener multilineas de código:
```js
const saludar = (nombre) => {
    console.log(`Hola ${nombre}, cómo estás?`) // --> envolvemos la variable entre ${}
}

saludar('Rodrigo') // --> 'Hola Rodrigo, cómo estás?'

// También podemos realizar operaciones
const suma = (a, b) => {
    console.log(`La suma entre ${a} y ${b} es igual a ${a + b}`)
}

suma(2, 4) // --> La suma entre 2 y 4 es igual a 6
```
## Codeemos un ratito
### Hagamos pruebas en el navegador
1. Abrir browser (Chrome, Firefox, Edge, Brave, etc)
2. Presionar F12 o clic derecho en _Inspeccionar Elemento_
3. Ir a la pestaña _Consola_
4. Pegar este código y darle a enter:
```js
const nombre = prompt('¿Cómo te llamás?') // Esto abrirá una pop-up en tu navegador en donde podes escribir
```
5. Después usa ```console.log(`Hola ${nombre}`)```.

### Sigamos con las pruebas
También probá esto:
1. Declará tu nombre y edad usando let y const. Podés usar una y una o intercarlar:
```js
let nombre = 'Rodrigo'
const edad = 27

console.log(`Hola, mundo ${nombre}? y tengo ${edad}`)
```
2. Probá cambiar valores. Si usaste const, pudiste cambiar el valor?
3. Usá 3 variables (_nombre_, _mascota_ y _ciudad_) y mostrá el texto por consola. Tendría que quedarte así: ```Hola, soy Rodrigo. Mi mascota es un/a gata y vivo en Neuquén```.

### Práctica para razonar
En el browser colocá:
```js
console.log(a)
console.log(b)

var a = 5
let b = 2
```
Qué pasó?

### Qué tipo de dato es?
Usá typeof() para saber qué tipo de dato es cada uno de los siguientes valores. Después, confirmá tus respuestas ejecutando el código en la consola
```js
typeof 123
typeof "hola"
typeof true
typeof undefined
typeof null
typeof NaN
typeof {}
typeof []
typeof function() {}
let ejemplo = "1234" // --> qué retorna? usa console.log
```

### Conversión de C° a F°
Una app para calcular los grados de Celsius a Fahrenheit:
```js
const c = 25
const f = (c * 9/5) + 32
console.log(`${c}°C son ${f}°F`) // --> '25°C son 77°F'
```
Ahora hacelo vos pidiéndole al usuario que ingrese los grados en Celsius y transformalos a Reaumur. El resultado mostralo por consola.
 
