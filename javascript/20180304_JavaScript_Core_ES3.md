# JavaScript The Core
This is my personal note on the blog post [JavaScript The Core](http://dmitrysoshnikov.com/ecmascript/javascript-the-core)

## An Object
An object is a _collection of properties_ and has a _single prototype object_.
```
let foo = {
  x: 10,
  y: 20,
}
```

Represented in a diagram:

![image of an object](http://dmitrysoshnikov.com/wp-content/uploads/basic-object.png)

`foo` object has two properties, `x` with value `10` and `y` with value `20`. Also, it has a reference to prototype object `__proto__`, which in this case is a default prototype object `Object.prototype`.

## A Prototype Chain
Prototype object is also a simple object with its own prototype object. If prototype reference is not `null`, a chain of prototype objects can be constructed with an object.
```
let foo = {
  x: 10,
  y: 20,
  // __proto__: Object.prototype, (omitted)
}

let bar = {
  i: 30,
  j: 40,
  __proto__: foo,
}
```
`foo` has default prototype object, and `bar` has a reference to `foo` as a prototype object.
If a property or method is not found in the object, an attempt is made to find them in the prototype chain. The value of first matching name is returned. Otherwise, `undefined` is returned instead.

## Constructor
Constructor function:
* Object creation with specified pattern
* Automatically sets prototype object for newly created object
```
// constructor function
function Foo(y) {
  this.y = y
}

Foo.prototype.x = 10
Foo.prototype.calculate = function (z) {
  return this.x + this.y + z
}

// create new objects with "Foo" pattern
let b = new Foo(20)
let c = new Foo(30)

// newly created objects inherits methods from constructor function
b.calculate(30) // 10 + 20 + 30 = 60
c.calculate(40) // 10 + 30 + 40 = 80
```

Above code can be represented in diagram below:

![image of constructor function](http://dmitrysoshnikov.com/wp-content/uploads/constructor-proto-chain.png)

## Execution Context Stack
Three types of ECMAScript code and every code is executed in its execution context:
* Global code (only one global context)
* Function code (there may be multiple instances of function context)
* _eval_ code

An execution context can activate another context (a function calls another function), and it's implemented as a stack, which is called **execution context stack**

Execution flow:
* _Caller_ is an execution context which activates another context.
* _Callee_ is an execution context which is activated by the _caller_.
* When _caller_ activates a _callee_, is is pushed to the execution context stack and becomes active execution context.
* After the _callee_ context ends, _callee_ is popped from the execution context stack, and returns control to _caller_.

All the ECMAScript program runtime is represented as the execution context stack.

## Execution Context
An execution context can be abstractly represented as an object:

![image of an execution context object](http://dmitrysoshnikov.com/wp-content/uploads/execution-context.png)

## Variable Object
_Variable object_ is a special object which stores variable and function definitions in the execution context. Notice, _function expressions_ are not stored as a part of variable object. _Variable object_ is an abstract concept where it's implemented in different ways in different execution context.

Variable object implementations:
* In global context: as global object
* In function context: as activation object

## Activation Object
When a function execution context is activated, a special object called **activation object** is created, and it's initialized with passed function parameters and `arguments` object.
```
function foo(x, y) {
  var z = 30
  function bar() {}
  (function baz() {})
}

foo(10, 20)
```

Activation Object for `foo` function execution context:

![image of foo activation object](http://dmitrysoshnikov.com/wp-content/uploads/activation-object.png)

## Scope Chain
Similar to _prototype chain_, if an identifier (variable, function or passed function parameter) is not found in current context's variable object (activation object), lookup continues in the parent's variable object, and so on.
```
var x = 10;
 
(function foo() {
  var y = 20;
  (function bar() {
    var z = 30;
    // "x" and "y" are "free variables"
    // and are found in the next (after
    // bar's activation object) object
    // of the bar's scope chain
    console.log(x + y + z);
  })();
})();
```

Note that _scope chain_ can be augmented with a simple object using `with` statement. In this case, prototype chain will be in consideration for looking up an identifier. `__proto__` will be looked up first before continuing on `__parent__`. Consider following example:
```
Object.prototype.x = 10;
 
var w = 20;
var y = 30;
 
// in SpiderMonkey global object
// i.e. variable object of the global
// context inherits from "Object.prototype",
// so we may refer "not defined global
// variable x", which is found in
// the prototype chain
 
console.log(x); // 10
 
(function foo() {
 
  // "foo" local variables
  var w = 40;
  var x = 100;
 
  // "x" is found in the
  // "Object.prototype", because
  // {z: 50} inherits from it
 
  with ({z: 50}) {
    console.log(w, x, y , z); // 40, 10, 30, 50
  }
 
  // after "with" object is removed
  // from the scope chain, "x" is
  // again found in the AO of "foo" context;
  // variable "w" is also local
  console.log(x, w); // 100, 40
 
  // and that's how we may refer
  // shadowed global "w" variable in
  // the browser host environment
  console.log(window.w); // 20
 
})();
```

With a diagram:

![image of augmented scope chain](http://dmitrysoshnikov.com/wp-content/uploads/scope-chain-with.png)

## Closure
In ECMAScript, functions can be passed to other functions as arguments. Functions receiving functions as arguments are called _higher order functions_. Also functions can be returned from other functions, and called _function valued functions_.

Two problems with _functional arguments_ AKA _funargs_ (Funarg problem):
* Upward funarg problem: When a function which is returned from another function uses _free variables_ from its parent context. To be able to access variables of parent context, _even after the parent context ends_, the inner function at creation moment saves parent's scope chain. Then when the returned function is activated, the scope chain of its context is formed as combination of the activation object and the saved parent's scope chain.
```
function foo() {
  var x = 10;
  return function bar() {
    console.log(x);
  };
}
 
// "foo" returns also a function
// and this returned function uses
// free variable "x"
 
var returnedFunction = foo();
 
// global variable "x"
var x = 20;
 
// execution of the returned function
returnedFunction(); // 10, but not 20
```

* Downward funarg problem: parent context exists, but may be an ambiguity with resolving an identifier. Which scope should be used when resolving identifier? ECMAScript _static scope_ is used via closure (saved parent's context at the creation of the function)
```
// global "x"
var x = 10;
 
// global function
function foo() {
  console.log(x);
}
 
(function (funArg) {
 
  // local "x"
  var x = 20;
 
  // there is no ambiguity,
  // because we use global "x",
  // which was statically saved in
  // [[Scope]] of the "foo" function,
  // but not the "x" of the caller's scope,
  // which activates the "funArg"
 
  funArg(); // 10, but not 20
 
})(foo); // pass "down" foo as a "funarg"

```

True definition of _closure_: A _closure_ is a combination of a function context scope and statically/lexically saved all parent scopes. Thus via these saved scopes a function may easily access free variables.

**NOTE:** several functions may have the same parent scope and variables in the same parent scope are _shared_ between those functions. Therefore, if variables in the parent scope is modified in one functions, it is reflected in another function which shares the same parent scope.
```
function baz() {
  var x = 1;
  return {
    foo: function foo() { return ++x; },
    bar: function bar() { return --x; }
  };
}
 
var closures = baz();
 
console.log(
  closures.foo(), // 2
  closures.bar()  // 1
);
```

Above code is represented in a diagram:

![image of shared parent scope](http://dmitrysoshnikov.com/wp-content/uploads/shared-scope.png)

## _this_ value
A _this_ value is a special object related with the execution context. It is a property of the execution context, not a property of the variable object. Also, it never participates in any identifier resolution process. When _this_ is referred in a code, its value is taken directly from the execution context, without any scope chain lookup. 

For a global context, the value of _this_ is global object.
For a function context, the value may be different depending on how the function is activated.
More details in another chapter...
