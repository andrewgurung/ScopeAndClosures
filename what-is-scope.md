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
