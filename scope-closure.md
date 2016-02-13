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
