# You Don't Know JS: `this` & Object Prototypes
You Don't Know JS Series ([here](https://github.com/getify/You-Dont-Know-JS))

## Chapter 1: `this` or That?
* Confusions
  * `this` is not the function itself
  * `this` is not referring its lexical scope
* What is `this`?
  * It's not an author-time binding, but a runtime binding
  * It's contextual based on the function's invocation
  * It has nothing to do with where a function is declared, rather when a function is invoked
  * When a function is invoked, an activation record (execution context) is created
  * An activation record contains information about:
    * where the function was called from (the call stack)
    * how the function was invoked
    * what parameters are passed
    * etc.

## Chapter 2: `this` All Makes Sense Now!
* Call-site: the location in code where a function is invoked, not where it's declared
* Default binding
  * Standalone function invocation
  ```
  function foo() {
    console.log(this.a)      // 'this' refers to global object
    // console.log(this.a)   // 'this' would be 'undefined' in strict mode
  }
  var a = 2
  foo() // 2
  ```
* Implicit binding
  * Call-site has a context object
  ```
  function foo() {
    console.log(this.a) // 'this' refers to 'obj'
  }

  var obj = {
    a: 2,
    foo: foo
  }

  obj.foo() // 2 
            // Here, 'obj' is the context object of foo
  ```
  * Implicit binding could be lost
  ```
  function foo() {
    console.log(this.a) // Here, 'this' refers to global object
  }

  var obj = {
    a: 2,
    foo: foo
  }

  var bar = obj.foo // function reference/alias!

  var a = 'oops, global' // 'a' also property on global object

  bar() // "oops, global"
  ```
  or when passing a callback function
  ```
  function foo() {
    console.log(this.a)
  }

  function doFoo(fn) {
    // 'fn' is just another reference to 'foo'

    fn() // <-- call-site!
  }

  var obj = {
    a: 2,
    foo: foo
  }

  var a = 'oops, global' // 'a' also property on global object

  doFoo(obj.foo) // 'oops, global'
  ```
* Explicit binding
  * Force a function call to use particular object for `this` binding
  * `call`, `apply` and `bind`
  * `call` and `apply` are hard-binding, meaning the bound `this` won't change later on
  
* `new` binding
  * What happens when a function is invoked with `new`:
    * A brand new object is created
    * The newly constructed object is [[Prototype]]-linked
    * The newly constructed object is set as the `this` binding for that function call
    * Unless the function returns its own alternate object, it automatically return the newly constructed object
    ```
    function foo(a) {
      this.a = a
    }
    var bar = new foo(2)
    console.log(bar.a)    // 2
    ```
* Binding precedence
  * `new` > explicit (`call`, `apply`, `bind`) > implicit (context object) > default
  * Exception: when `call`, `apply`, `bind` are called with `null` or `undefined` => fall back to default binding

* Lexical `this` binding
  * ES6 arrow function: `this` refers to the `this` of immediate enclosing scope
  * Essentially, `self = this`

## Chapter 3: Objects
* `for..of`: loop on an Iteraator
* `for..in`: loop on keys of an object
* Automatic string coercion
```
var str = 'I am a string'  // string literal
console.log(str.length)    // 13 (string literal is coerced to String object, which has a function length())
console.log(str.charAt(3)) // "m" (also coercion happens to perform charAt() on String object)
```
* Automatic number coercion
```
var num = 42.359
console.log(num.toFixed(2)) // 42.36 (number literal is coerced to Number object to perform toFixed())
```
* For most use cases, simple literal form is preferred. Only use the constructed form only if necessary

## Chapter 4: "Class" Objects

## Chapter 5: Prototypes
