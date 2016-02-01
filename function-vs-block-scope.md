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
