## Hiding in Plain scope
* You can hide variables and functions by enclosing them in the scope of a function.  
* Inspired by the Principle of Least Privilege which states that you should expose only minimally and hide everything else.
* Another benefit of hiding variables and functions is to avoid unintended collision of identifiers.

```js
function doSomething(a) {
  b = a + doSomethingElse( a * 2 );
  console.log( b * 3 );
}

function doSomethingElse(a) {
  return a - 1;
}

var b;

doSomething( 2 ); // Output 15
```

The b variable and doSomethingElse(..) can be treated as private details of doSomething(..) function.

**Proper Design**

```js
function doSomething(a) {
  function doSomethingElse(a) {
    return a - 1;
  }

  var b;

  b = a + doSomethingElse( a * 2 );

  console.log( b * 3 );

}

doSomething( 2 ); // Output 15
```

## Global Namespace
Multiple libraries can collide with each other. To avoid such collision, such libraries will create a single variable declaration (often an object) with significantly unique name, in the global scope.  

This object is then used as a **namespace** for that library, where all specific exposures of functionality are made as properties off that object.

```js
var MyBestJSLibrary = {
  awesome: "stuff",
  doSomething: function() {
    //..
  },
  doAnotherThing: function() {
    //..
  }
};
```

## Function as Scope
Function can enclose any variable or function declaration by wrapping it inside its inner function scope.  
But can lead to global namespace pollution and explicitly call the named function (Eg. foo()) to execute it.

```js
var a = 2;
function foo() {
  var a = 3;
  console.log(a);
}
foo(); //3
console.log(a); //2
```

**Better alternative:**

1. No function name (or rather doesn't pollute global namespace)
2. Function will be automatically executed

```js
var a = 2;
(function foo() {
  // foo identifier is only accessible inside of foo() {..} function
  // Hence foo doesn't pollute global namespace
  var a = 3;
  console.log(a);
})(); //3
console.log(a); //2
```

(function foo() {..}) is a function expression which is followed by parenthesis () to execute it.

> If function is the first thing of a statement, then it's function declaration. Else it's function expression.

### Anonymous Vs Named Function

```js
setTimeout( function(){
  console.log("I waited 5 second!");
}, 5000 );
```

This is called an anonymous function expression, because function(){..} has no name identifier on it.

1. No useful name to display in stack trace.
2. Hard to refer to. Must use deprecated arguments.callee to self-reference

Best Practice: Always name your function expressions

```js
setTimeout( function timeoutHandler(){
  console.log("I waited 5 second!");
}, 5000 );
```


### Immediately Invoked Function Expression

1. Function as an expression by wrapping with (). Eg (function foo(){..})
2. Invoked immediately by (). Eg (function foo(){..})()
3. Though IIFEs are anonymous, its suggested to name your IIFEs
4. Variation of IIFE. (function foo(){..}()) Works same as #1. Simply a matter of stylistic choice

```js
var a = 2;

(function foo() {

  var a = 3;
  console.log( a ); //3

})();

console.log( a ); //2
```

## Block as Scopes
On the surface, Javascript has no facility for block scope. Not until you dig further.
1. with
  - `with` keyword is a shorthand for making multiple property references against an object
  - `with` keyword creates a whole new lexical scope by treating an object reference as a scope and that object’s properties as scoped identifiers
  - The scope created from the object only exists for the lifetime of that with statement

2. try/catch
  - The variable declaration in the catch clause is block scoped
  ```js
  try {
      undefined();
  }
  catch (err) {
    console.log( err ); //Correctly displays the error message
  }
  console.log( err ); //ReferenceError: err is not defined
  ```

3. let
  - ES6 introduces `let` keyword
  - The scope of variables declared using let keyword is limited within the containing block (commanly a {..} pair)

  ```js
  var foo = true;

  if (foo) {
    let bar = foo * 2;
    console.log( bar ); // 2
  }

  console.log( bar ); // ReferenceError: bar is not defined
  ```
  Best Practice: Use Explicit let block to avoid error while moving code during code refactor in future

  ```js
  var foo = true;

  if (foo) {
    { // <-- Explicit block
      let bar = foo * 2;
      console.log( bar ); // 2
    }
  }

  console.log( bar ); // ReferenceError: bar is not defined
  ```

  Note: let will not hoist to the entire scope of the block
  ```js
  {
    console.log(foo);
    let foo = 'Hoist'; // ReferenceError: can't access lexical declaration `foo' before initialization
  }
  ```

  ### Garbage Collection ###
  Block-scoping makes it clear to the engine that it does not need to keep someReallyBigData around which tells garbage collection to reclaim memory
  ``` js
  function process(data) {
      // do something interesting
  }

  // anything declared inside this block can go away after!
  {
      let someReallyBigData = { .. };

      process( someReallyBigData );
  }

  var btn = document.getElementById( "my_button" );

  btn.addEventListener( "click", function click(evt){
      console.log("button clicked");
  }, /*capturingPhase=*/false );
  ```

  ### `let` loop ###
  Use case of `let`

  ```js
  for(let i = 0; i < 5; i++) {
    console.log( i ); // 0 1 2 3 4
  }

  console.log( i ); // ReferenceError
  ```
4. const
  - ES6 introduces `const` keyword
  - `const` creates block-scoped variable whose value is fixed
  - Trying to change the value of a `const` will result in error

  ```js
  var foo = true;

  if (foo) {
    var a = 2;
    const b = 3;

    a = 5; // Valid assignment
    b = 5; // Error
  }

  console.log( a ); // 5
  console.log( b ); // ReferenceError
  ```
