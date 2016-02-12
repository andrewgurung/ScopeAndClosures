## Hoisting

* All function and variable declarations are processed first by the compiler.
* Declarations are processed in compilation phase, and assignment is left in place.
* Hoisting metaphorically means moving function and variable declaration to the top of the code.

1.a Hoisting example

```js
a = 2;
var a;
console.log( a ); // 2
```

1.b How the engine treats it.
```js
var a;
a = 2;
console.log( a ); // 2
```

2.a Hoisting example

```js
console.log( a ); // undefined
var a = 2;
```

2.b How the engine treats it.
```js
var a;
console.log( a ); // undefined
a = 2;
```

* Function Expression aren't hoisted.

```js
// Only foo variable is hoisted, but the function isn't.
// foo is undefined
// Trying to execute foo() on an undefined variable will result in TypeError

foo(); // not ReferenceError, but TypeError

var foo = function bar() {
  console.log( "Bar" );
};
```
