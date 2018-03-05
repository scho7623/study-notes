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



