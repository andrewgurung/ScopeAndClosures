## Hoisting

* Hoisting metaphorically means moving function and variable declaration to the top of the code.
* All function and variable declarations are processed first by the compiler.
* Declarations are processed in compilation phase, and assignment is left in place.


### Hoisting variable

```js
a = 2;
var a;
console.log( a ); // 2
```

***How the engine interprets it:***

```js
var a;
a = 2;
console.log( a ); // 2
```

### Hoisting variable but assignment in place

```js
console.log( a ); // undefined
var a = 2;
```

***How the engine interprets it:***

```js
var a;
console.log( a ); // undefined
a = 2;
```

### Hoisting function

```js
display(); // Hoisting function!

function display() {
  console.log( "Hoisting function!" ); // Hoisting function!

  console.log( a ); // undefined

  var a = 100;
}
```

***How the engine interprets it:***

```js
function display() {
  var a;

  console.log( "Hoisting function!" ); // Hoisting function!

  console.log( a ); // undefined

  a = 100;
}

display(); // Hoisting function!
```

### Function Expression aren't hoisted

```js
// Only foo variable is hoisted, but the function isn't.
// foo is undefined
// Trying to execute foo() on an undefined variable will result in TypeError

foo(); // not ReferenceError, but TypeError
bar(); // Reference Error

var foo = function bar() {
  console.log( "Bar" );
};
```

***How the engine interprets it:***

```js
var foo;
foo(); // TypeError
bar(); // ReferenceError

foo = function() {
  // bar can only be referenced by the codes inside here.
  var bar = ...self...
  // ...
}
```

### Functions are hoisted before variables

```js
foo(); // bar

function foo() {
    console.log( "foo" );
}

function foo() {
    console.log( "bar" );
}

var foo = function() {
    console.log( "baz" );
};

foo(); // baz
```

***How the engine interprets it:***

```js
//   Function Overridden
//   function foo() {
//      console.log( "foo" );
//    }

function foo() {
    console.log( "bar" );
}

foo(); // bar

foo = function() {
    console.log( "baz" );
};

foo(); // baz
```
