# You Don't Know JS: Scope & Closures
You Don't Know JS Series ([here](https://github.com/getify/You-Dont-Know-JS))

## Chapter 1: What is Scope?
* Scope: A well-defined set of rules for storing variables and for finding those variables at a later time
* JavaScript Engine Compilation Steps
  * Tokenizing/Lexing: Generate tokens
  * Parsing: Build AST (Abstract Syntax Tree)
  * Code-generation: Take AST and generate executable code
* Engine, Compiler, Scope play different roles
* Variable look up is done from current scope to global scope

## Chaper 2: Lexical Scope
* Two models how scope works: _Lexical scope_ and _dynamic scope_
* Lexing: Examining a string of **source code** characters and assign semantic meanings
* Lexical scope: A scope that is defined in **lexing** time

## Chapter 3: Function vs Block Scope
* Function scope: Each function declared creates a new scope
* Function name of named function expression is only available within the function scope
```
(function foo() {
  console.log(foo)  //  function
})()
console.log(foo)  // ReferenceError error
```
* Block scope: A new scope is created within curly braces `{...}`
* `let`, `const`: attaches the variable declaration to the current block scope only
* Variable hoisting does not apply to `let` or `const`

## Chapter 4: Hoisting
* Only declaration is hoisted to the top of the scope
* Variables (`var`) and function declarations are hoisted
* Function expressions are **NOT** hoisted

## Chaper 5: Scope closures
* _Closure is when a fucntion is able to remember and access its lexical scope even when that function is executing outside its lexical scope_
```
function wait(message) {
  setTimeout(function() {
    console.log(message)
  }, 1000)
}

wait('this is a message!')  // log the message even though wait() function finishes executing
```

## Appendix A: Dynamic scope
* Dynamic scope: a scope is determined dynamically at runtime
* JavaScript doesn't have dynamic scope, it has lexical scope
* `this` keyword: Makes JavaScript kind of like dynamic scope

## Appendix C: Lexical `this`
* `this` binding in arrow functions (ES6):
  * Arrow functions discard all the normal rules for `this` binding
  * Instead, arrow functions take `this` as value of their immediate lexical enclosing scope
  * In other words, arrow functions inherit `this` from its lexical scope
