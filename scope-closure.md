## Closure

> Closure is when a function can remember and access its lexical scope
even when it’s invoked outside its lexical scope.

Anytime a function is returned as a value and invoked in other location, we are observing closure.

### Code Sample

```js
function foo() {
  var result = "Closure";

  function bar() {
    console.log(result);
  }

  return bar;
}

var baz = foo();

baz();
```

- Function `bar()` has lexical scope access to the inner scope of `foo()`
- After executing var baz = foo(), we expect garbage collection to clear content of function `foo()`
- But `bar()` has lexical scope closure over that inner scope of function `foo()` which keeps that scope alive for `bar()` to reference at later
- `bar()` having reference to that scope is called closure
- Function is invoked outside of author-time lexical scope

### Passing inner function to global function

```js
function foo() {
  var a = "Closure";

  function closed() {
    console.log( a );
  }

  bar( closed );
}

function bar(fn) {
  fn();
}

foo(); // Closure
```

### Assigning inner function to global variable

```js
var fn;

function foo() {
  var a = "Closure";

  function closed() {
    console.log( a );
  }

  fn = closed;
}

function bar() {
  fn();
}

foo();

bar(); // Closure
```

### Closure around us

```js
function wait(message) {
  setTimeout(function closed(){
    console.log( message );
  }, 1000);
}

wait( "Closure" );
```

- Similar to "Passing inner function to global function"
- `setTimeout()` is a library method with an implementation that takes two parameters: function(){..} and time
- We pass the inner function `closed()` to `setTimeout()`, but `closed()` has scope reference over `wait()` which can access `message` identifier from the internal function parameter fn(){..} of `setTimeout()`

### Loops and Closure

```js
for (var i=1; i<=5; i++) {
  setTimeout( function timer(){
  console.log( i );
}, 0 );
}
/*
Output: Instead of 1 2 3 4 5, it prints the following
6
6
6
6
6
*/
```

- Linters often complain when you put functions inside loops
- The loop stops when the value of i = 6. So final value of printed `i` is 6
- Even if the timeout is set to 0 millisecond, the timeout function callbacks will run well after the completion of the loop
- Though these 5 functions are defined separately in each iteration, they are closed over the same shared global scope

### Using IIFE to solve use of function in loops

```js
for (var i=1; i<=5; i++) {
  (function (j) {
      setTimeout( function timer(){
      console.log( j );
    }, 0 );
  })(i);
}
```
- Use of IIFE created a new scope for each iteration
