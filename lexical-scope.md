## Lexical Scope

There are two predominant models for scoping.  
1. Lexical Scope (Most programming languages including JavaScript)
2. Dynamic Scope (Some programming languages such as Bash scripting)

### Compiler theory
There are three steps before a code is executed, roughly called 'compilation':

1. Lexing / Tokenizing
2. Parsing
3. Code-Generation

>Lexing is the process of breaking up a string of characters into meaningful (to the language) chunks, called tokens.

Lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is (mostly) set in stone by the time the lexer processes your code.

<b>Best Practice: </b>Although you can cheat lexical scoping, it is considered best practice to treat lexical scope as, in fact, lexical-only.

```js
function foo(a) {
  var b = a * 2;

  function bar(c) {
    console.log( a, b, c);
  }

  bar ( b * 3 );
}

foo( 2 ); // Output: 2 4 12
```

There are three nested scopes in this example which can be thought as bubbles as shown below:  
![Nested Scopes](fig2.png "Nested Scopes")

Bubble 1 encompasses the global scope and has just one identifier in it: foo.  
Bubble 2 encompasses the scope of foo, which includes the three identifiers: a, bar, and b.  
Bubble 3 encompasses the scope of bar, and it includes just one identifier: c.

#### Look-ups
The Engine executes console.log() and goes looking for references to a, b and c. It starts from the innermost bubble, the scope of bar function. If it doesn't find it there, it goes outside to foo function and eventually to the global scope.

>No matter where a function is invoked from, or even how it is invoked, its lexical scope is only defined by where the function was declared.

### Cheating Lexical Scope
JavaScript has two methods of cheating lexical scope:

1. eval(..)
2. with

Bad Practice: Cheating lexical scope leads to poorer performance.

#### eval
* The eval(..) function in JavaScript takes string argument and treats the content as if it had been there at author time.
* eval(..) can modify existing lexical scope by evaluating a string of 'code' that has one or more declarations in it.

  ```js
  function foo(a, str) {
    eval(str); // Declares a new local variable b = 45 by modifying the lexical scope
    console.log( a, b ); // Ignores global var b = 2
  }

  var b = 2;

  foo(1, 'var b = 45'); //Output: 1 45
  ```

  Note: The string of code 'var b = 45' that we passed in was a fixed literal but eval(..) can also execute dynamically created code.

  #### Strict mode
  eval(..) when used in strict mode do not modify the enclosing scope

  ```js
  function foo(str) {
    'use strict'
    eval(str);
    console.log( a );
  }

  foo('var a = 100'); // ReferenceError
  ```

  ####Note:
  * setTimeout() and setInterval() functions are similar to eval(). But both of them are deprecated. Don't use it.
  * new Function() constructor is slightly safer than eval() but it should still be avoided.
