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
