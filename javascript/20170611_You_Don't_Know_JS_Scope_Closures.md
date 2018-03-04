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
* Function scope vs block scope
  * `var` does not support block level scope
  * Use `let` to create block scope variable

## Chaper 2: Lexical Scope
* Two models how scope works: _Lexical scope_ and _dynamic scope_
* Lexing: Examining a string of **source code** characters and assign semantic meanings
* Lexical scope: A scope that is defined in **lexing** time
* Resolution of an identifier is independent from the environment of a **caller**

## Chaper 2.1: `with` and `eval`
* JavaScript **does not** support dynamic scope
* However, `with` and `eval` are "like" dynamic scope, just because it's _impossible_ to determine at lexing time
* With `strict` mode, `with` and `eval` may not work as expected
* General rule of thumb: **Don't use `with` or `eval`**

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
* _[Sungho]_: In my opinion, `hoisting` is a little deceptive, as JavaScript engine is actually **NOT** reorganizing the code to declare variables and functions to the top of the scope. Instead, it's a two step process:
  * Execution context build
    * In function context: Activation Object (AO) is created on entering the function context with `arguments` property initialized with following:
      * _callee_: reference to the current function
      * _length_: number of really passed arguments
      * _properties-indexes (integer converted to string)_: values of arguments (left to right, starting from index 0)
        * Values of _properties-indexes_ and **really passed** formal parameters are shared
        ```
        function foo(x, y, z) {
          alert(foo.length) // defined function arguments: 3
          alert(arguments.length) // really passed arguments: 2
          
          // parameter sharing
          alert(x === arguments[0]) // true
          alert(x) // 10
          arguments[0] = 20
          alert(x) // 20
          x = 30
          alert(arguments[0]) // 30
        }
        
        foo(10, 20)
        ```
  * Code execution

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
