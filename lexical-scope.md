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
