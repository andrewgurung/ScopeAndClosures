## What is scope?
A well-defined set of rules for  storing variables in some location, and for finding those variables at a later time.

>Although JavaScript falls under the general category of “dynamic” or “interpreted” languages, it is in fact a compiled language.

## Understanding Scope
Learning about scope through conversation.

### The Cast
1. Engine: Responsible for start-to-finish compilation and execution of our JavaScript program.
2. Compiler: One of Engine’s friends; handles all the dirty work of parsing and code-generation.
3. Scope: Another friend of Engine; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

### Back and Forth
Break down of how Engine and friends will approach the program
```js
 var a = 2;
 ```
Distinct actions:
1. Compiler declares a variable (if not previously declared) in the current Scope.
2. Compiler then produces code 'a = 2' assignment for Engine to handle. When executing, Engine looks up the variable in Scope(including nested scope) and assigns to it, if found (else throw an error)

### Compiler Speak
When executing the above code sample, Engine looks up for the variable 'a' in Scope.
There are two types of look-up:
1. LHS (lefthand side of an assignment)
  * Who’s the target of the assignment?
  * When a variable appears on the lefthand side of an assignment operation
  * Tries to find the variable container itself, so that it can assign

  ```js
  /* We don’t actually care what the current value is, we simply want to find the variable as a target for the = 2 assignment operation. */
  a = 2
  ```
2. RHS
  * Who’s the source of the assignment?
  * Not LHS
  * Simply find the value of some variable
  * Can be thought as 'retrieve his/her value'

  ```js
  /* Nothing is being assigned to 'a' here. Instead, we’re looking up to retrieve the value of 'a', so that the value can be passed to console.log(..) */
  console.log(a);
  ```

#### Both LHS and RHS references

```js
function foo(a) { // Implicit assignment (a = 2). Thus LHS look up to 'a'
  console.log( a ); // RHS references to 'a' variable and 'console' object
}

foo( 2 ); // RHS reference to foo function

// Output: 2
```

### Engine/Scope conversation
```js
function foo(a) {
  console.log( a );
}

foo( 2 );
```

##### Learning about scope through conversation:

Engine: Hey Scope, do you have a RHS reference for 'foo'?  
Scope: Yes I do. Compiler declared it as a function. Here you go.  
Engine: Scope, do you have an LHS reference for 'a'?  
Scope: Yes, Compiler declared it as a formal parameter to 'foo'. Here you go.  
Engine: Time to assign '2' to 'a'  
Engine: Any RHS reference for console?  
Scope: It's a built in object. Here you go.  
Engine: Looking up .log(). Oh it's a function.  
Engine: I think I remember but double checking RHS reference to 'a'.  
Scope: You are right. 'a' hasn't changed. Here you go.  
Engine: Cool. Executing console.log() by passing value of 'a' which is '2'

## Nested Scope  
If a variable cannot be found in the immediate scope, Engine consults the next outer containing scope, continuing until is found or until the outermost/global scope has been reached. You either find what you’re looking for, or you don’t. But you
have to stop regardless.

``` js
function foo(a) {
  /*
  Learning through conversation again:
  Engine: Hey scope of 'foo', do you have a RHS reference for 'b'.
  Scope(foo): No, go fish outside.
  Engine: Hey outer scope of foo (Global scope in this case), do you have RHS reference to 'b'?
  Scope(Global): Yes. Here you go '3'
  */
  console.log( a + b);

}

var b = 3;

foo( 2 ); // Output: 5
```

## Errors

1. RHS error: Couldn't find RHS reference for 'b' in any scope

  ```js
  function foo(a) {
    console.log( a + b); //Couldn't find RHS reference for 'b' in foo scope nor global scope
    b = a;
  }

  foo( 2 ); // Reference Error
  ```

  <b>Solution:</b>
  ```js
  function foo(a) {
    console.log( a + b);
  }
  var b = 2;
  foo( 2 ); // Output: 4
  ```

2. LHS error: Couldn't find LHS reference for 'b' in global space due to strict mode turned ON

  ```js
  "use strict"
  function foo(a) {
    /*
    While trying to perform LHS b = a, we must first find the variable container 'b'.
    Since it cannot find it inside foo scope, it goes out to global scope.
    If program is NOT running in strict mode, the global scope will create a new variable
    of 'b' in the global scope, and hand it back to Engine.

    But strict mode disallows the automatic/implicit global variable creation.
    Hence it throws a Reference Error similar to RHS error in example 1

    */
    b = a;
    console.log( a + b);

  }

  foo( 2 ); // Reference Error
  ```
  <b>Solution:</b>
  ```js
  "use strict"
  function foo(a) {
    var b = a;
    console.log( a + b);

  }

  foo( 2 ); // Output: 4
  ```

3. Type Error:  Trying to execute as function a non-function value, or reference a property on a null or undefined value results in Type Error

  ```js
  // Example: Executing as function a non-function value
  function foo(a) {
    a();
  }

  foo( 2 ); // TypeError
  ```

  ```js
  // Example: Reference a property on an undefined value
  function displayLength(a) {
    console.log( a.length );
  }

  var arr1 = ['a', 2, true];
  displayLength( arr1 ); // Output: 3

  var arr2;
  displayLength( arr2 ); // TypeError
  ```

>ReferenceError is scope resolution-failure related, whereas TypeError implies that scope resolution was successful, but that there was an illegal/impossible action attempted against the result.
