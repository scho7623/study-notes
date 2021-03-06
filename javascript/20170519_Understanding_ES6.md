# Understanding ES6 by Nicholas Zakas

## Chapter 1: Block Bindings
* Hoisting: Variables declared with `var` are treated as if they are at the top of the function
* ES6 introduces **block-level declaration** to provide more control over variable life cycle
  * Variables declared in a block scope are inaccessible outside the scope
  * Block scopes are created in
    * Inside a **function**
    * Inside a block (between `{` `}`)
* `let`: Similar to `var`, but only assessible in the current scope and won't be hoisted
* `const`: Constant which must be initialized on declaration and once declared, it cannot be reassigned
* Object declaration with `const`: Possible to modify object since it's not reassigning the `const` itself
```
const person = {}
person.name = 'Sungho'      // This is valid
person = { name: 'Sungho' } // This throws an error
```

* **Best practice**: Use `const` by default, only use `let` when you know a variable needs to be reassigned later!

## Chapter 2: Strings and Regular Expressions
* Surrogate pairs: A UTF-16 character that is represented with two 16-bit code units
* `codePointAt()`: Returns the Unicode code point at a given position
* `fromCodePoint()`: Returns a character corresponding to the given code point
* ES6 supports regular expression with `u` flag, which stands for Unicode: Use characters instead of code unit
* Regular expressions with `source` and `flags` property
* In addition to `indexOf()` to identify substring, following methods are added:
  * `includes(string, index)`: True if `string` found within the string searching from `index`
  * `startsWith(string, index)`: True if `string` found at the beginning of the string from `index`
  * `endsWith(string, index)`: True if `string` found at the end of the string searching from `length - index`
  * `repeat(n)`: Returns a new string repeating the original string `n` number of times

## Chapter 3: Functions
* **Default parameter values**: assign default value when the parameter isn't passed
```
function makeRequest(url, timeout = 2000, callback = function () {}) {
  // make a request with given timeout and callback
}
```
* In ES5 strict mode, the `arguments` object does not reflect changes to the named parameters
* Like ES5 strict mode, ES6 with default parameter does not change original `arguments` object
```
function someFunc(first, second = 'b') {
  console.log(arguments.lengt)         // 1
  console.log(first === arguments[0])  // true
  console.log(second === arguments[1]) // false
}
someFunc('a')
```
* Default parameter can be **any expression**, not just a primitive value
* **Rest parameter**
  * Indicated by three dots `...` in front of the named parameter
  * It becomes an array containing the rest of the parameters passed to a function
  * There can be only **one** rest parameter and it must be the **last** parameter
  * It cannot be used in **object literal setter**
* **Spread operator**: Split an array into separated arguments to be passed to a function
* **Arrow function**

## Chapter 4: Expanded Object Functionality
* Property initialization shorthand:
```
function createPerson(name, age) {
  return {
    name,
    age
  }
}
```
* Concise method:
```
const person = {
  name: 'Sungho',
  sayName() {
    console.log(this.name)
  }
}
```
* Computed property names:
```
const propName = 'location'
const person = {
  name: 'Sungho',
  [propName]: 'Palo Alto, CA'
}
console.log(person.location)  // 'Palo Alto, CA'
console.log(person[propName]) // 'Palo Alto, CA'
```
*`Object.is()`: Returns true if two arguments are the same (same type and same value)
```
console.log(Object.is(5, 5))     // true
console.log(Object.is(5, '5'))   // false
console.log(Object.is(+0, -0))   // false
console.log(Object.is(NaN, NaN)) // true
```
* `Object.assign()`: Assign properties of one object to another and returns the object with additional properties
  * With multiple suppliers, the latter supplier overwrites the properties with the same name
```
let receiver = {}
Object.assign(receiver, {
  name: 'Sungho',
  location: 'Mountain View, CA'
}, {
  location: 'Palo Alto'
})
console.log(receiver.location)   // 'Palo Alto'
```

## Chapter 5: Destructuring
* **Destructuring**: Process of breaking down a data structure (object/array) into smaller parts
* Object destructuring
  * Example:
  ```
  const person = {
    name: 'Sungho',
    location: 'Palo Alto, CA'
  }
  const { name, location } = person
  console.log(name)      // 'Sungho'
  console.log(location)  // 'Palo Alto, CA'
  ```
  * Destructuring assignment:
  ```
  name = 'Tom'
  location = 'Mountain View, CA'
  ({ name, location } = person)  // make sure to wrap with parenthesis
  console.log(name)              // 'Sungho'
  console.log(location)          // 'Palo Alto, CA'
  ```
  * Default values
  ```
  const { name, location, occupation } = person
  console.log(name)       // 'Sungho'
  console.log(location)   // 'Palo Alto, CA'
  console.log(occupation) // undefined
  
  ({ occupation = 'Software Engineer' } = person)
  console.log(occupation) // 'Software Engineer'
  ```
  * Assigning to different local variable names
  ```
  const { name: localName, location: localLocation, occupation: localOcc = 'Software Engineer' } = person
  console.log(localName)     // 'Sungho'
  console.log(localLocation) // 'Palo Alto, CA'
  console.log(localOcc)      // 'Software Engineer'
  ```
  * Nested object destructuring
  ```
  const person = {
    name: 'Sungho',
    skills: {
      javascript: [
        'ES6',
        'React',
        'Redux'
      ]
    }
  }
  const { skills: { javascript } } = person
  console.log(javascript.length)     // 3
  console.log(javascript.join(','))  // 'ES6','React','Redux'
  
  const { skills: { javascript: js } } = person  // Assigning to different local variable name is possible
  console.log(js.length)        // 3
  console.log(js.join(','))     // 'ES6','React','Redux'
  ```
* Array destructuring
  * Array destructuring works on positions rather than the named properties in object
  ```
  const javascript = [ 'ES6', 'React', 'Redux' ]
  const [ firstJS, secondJS ] = javascript
  console.log(firstJS)   // 'ES6'
  console.log(secondJS)  // 'React'
  ```
  * You can skip any variables you want to ignore by only providing variable names you are interested only
  ```
  const [ , , thirdJS ] = javascript
  console.log(thirdJS)   // 'Redux'
  ```
  * Swapping variables using array destructuring
  ```
  let a = 1
  let b = 2
  [ a, b ] = [ b, a ]
  console.log(a)        // 2
  console.log(b)        // 1
  ```
  * Nested array destructuring
  ```
  const skills = [ 'Java', 'C', ['ES6', 'React', 'Redux'] ]
  const [ firstSkill, secondSkill, [thirdSkill] ] = skills
  console.log(firstSkill)    // 'Java'
  console.log(secondSkill)   // 'C'
  console.log(thirdSkill)    // 'ES6'
  ```
  * **Rest Items**: Assign remaining items in an array
  ```
  const javascript = [ 'ES6', 'React', 'Redux' ]
  const [ firstSkill, ...restSkills ] = javascript
  console.log(firstSkill)           // 'ES6'
  console.log(restSkills.length)    // 2
  console.log(restSkills[0])        // 'React'
  console.log(restSkills[1])        // 'Redux'
  ```
  * Cloning with rest items
  ```
  const javascript = [ 'ES6', 'React', 'Redux' ]
  const [ ...cloneJS ] = javascript
  console.log(cloneJS)  // [ 'ES6', 'React', 'Redux' ]
  ```
* Mixed destructuring: Mix of object and array destructuring
```
const person = {
  name: 'Sungho',
  location: {
    street: 'University Ave',
    city: 'Palo Alto',
    state: 'CA'
  },
  skills: [
    'JavaScript',
    'Java',
    'C'
  ]
}
const {
  location: { city: localCity },
  skills: [ firstSkill ]
} = person
console.log(localCity)     // 'Palo Alto'
console.log(firstSkill)    // 'JavaScript'
```
* Destructured parameters
```
function setCookie(name, value, { secure, path, domain, expires } = {}) {
  // code to set the cookie
}

setCookie('type', 'js', {
  secure: true,
  expires: 60000
})
```

## Chapter 6: Symbols
* **Symbol**: A primitive type to create private object members
* Creating symbols:
```
const firstName = Symbol('first name')
const person = {}
person[firstName] = 'Sungho'
console.log(person[firstName])     // 'Sungho'
console.log(firstName)             // 'Symbol(first name)'
console.log(typeof firstName)      // 'symbol'
```
* Using symbols:
```
// use symbol in object literal
const firstName = Symbol('first name')
const person = {
  [firstName]: 'Sungho'
}

// make firstName property read-only
Object.defineProperty(person, firstName, { writable: false })

// use symbol in function argument
const lastName = Symbol('last name')
Object.defineProperties(person, {
  [lastName]: {
    value: 'Cho',
    writable: false
  }
})

console.log(person[firstName])    // 'Sungho'
console.log(person[lastName])     // 'Cho'
```
* Sharing symbols:
```
// create a symbol for the first time
const uid = Symbol.for('uid')
const object = {
  [uid]: '12345'
}

// retrieve a symbol from the registry
const uid2 = Symbol.for('uid')
console.log(uid === uid2)     // true
console.log(object[uid2])     // '12345'
console.log(uid2)             // 'Symbol(uid)'
```
* **Note**: Global symbol registry is a shared environment just like global scope! Use proper namespaces to avoid naming collision!
* Retrieving symbol properties from object:
  * `Object.keys()`, `Object.getOwnPropertyNames()` won't return symbol properties
  * `Object.getOwnPropertySymbols()` added in ES6 to retrieve symbol properties from the object
  ```
  const uid = Symbol.for('uid')
  const object = {
    [uid]: '12345'
  }
  const symbols = Object.getOwnPropertySymbols(object)
  console.log(symbols.length)     // 1
  console.log(symbols[0])         // 'Symbol(uid)'
  console.log(object[symbols[0])  // '12345'
  ```

## Chapter 7: Sets and Maps
* **Set**: A list of values that cannot cotain duplicates
  * Creating Sets and adding items
  ```
  const set = new Set()
  set.add(5)
  set.add('5')
  console.log(set.size)  // 2
  ```
  * Initializing Sets with array (or any iterable objects)
  ```
  const set = new Set([1, 2, 3, 4, 5, 5, 5, 5, 5])
  console.log(set.size)   // 5 (duplicate 5s are ignored)
  ```
  * Testing Sets if a value exists
  ```
  const set = new Set([5, '5'])
  console.log(set.has(5))   // true
  console.log(set.has(6))   // false
  ```
  * Removing items from Sets
  ```
  const set = new Set([5, '5'])
  console.log(set.has(5))   // true
  set.delete(5)
  console.log(set.has(5))   // false
  console.log(set.size)     // 1
  set.clear()
  console.log(set.has('5')  // false
  console.log(set.size)     // 0
  ```
  * `forEach()` method in Sets
    * Takes a callback function with three arguments
      * The value from the next position on the set
      * **The same value as the first argument**
      * The set from which the value is read
    * Why the second argument is the same as the first argument?
      * Map and Array also has `forEach()` method
      * They also take a callback function with three arguments
        * The value from the next position on the set
        * The key of the first argument, the value (numeric index in case of Array)
        * The map or array which the value is read from
      * Because Set does not have key, the second argument is set to the value to be consistent
    ```
    const set = new Set([1, 2, 3, 4, 5])
    set.forEach((value, key, ownerSet) => {
      console.log(value === key)      // true
      console.log(ownerSet === set)   // true
    })
    ```
  * Converting a Set to an Array: **Use spread operator(...)**
  ```
  const set = new Set([1, 2, 3, 3, 3, 3, 4, 5])   // duplicates are removed
  const array = [...set]
  console.log(array)     // [1,2,3,4,5]
  ```
  * **Weak Sets**: `new WeakSet()` creates a weak set which works exactly the same as Set, but can't contain nonobject values, like primitive values. It's useful when the object need to be garbage collected, if the only reference to the object is held at the weak set to properly handle memory. It does not have `forEach()` method nor `size` property.
* **Map**: A collection of keys that correspond to specific values
  * `set(key, value)` method to add key/value pairs into the map
  * `get(key)` method to retrieve value corresponding to the key
  ```
  const map = new Map()
  map.set('firstName', 'Sungho')
  map.set('lastName', 'Cho')
  console.log(map.get('firstName'))    // 'Sungho'
  console.log(map.get('lastName'))     // 'Cho'
  ```
  * `has(key)` method to determine if the key exists in the map
  * `delete(key)` method to remove the key and corresponding value
  * `clear()` method to remove all key/value from the map
  * `size` property to get the number of key/value pairs in the map
  * Map can be initialized with an array of arrays where each array has two items for key and value, respectively
  ```
  const map = new Map([['firstName', 'Sungho'], ['lastName', 'Cho']])
  console.log(map.has('firstName'))    // true
  console.log(map.get('firstName'))    // 'Sungho'
  console.log(map.size)                // 2
  ```
  * Weak Map: Similar to Weak Set

## Chapter 8: Iterators and Generators
* **Iterators** have `next()` method which returns a result object which has two properties
  * `value`: next value
  * `done`: boolean which is `true` when there are no more values to return
* A **Generator** is a function returns an iterator
  * Indicated by an asterisk character after `function` keyword
  * Use the new `yield` keyword: specifies the return value when `next()` is called
  * Execution stops once one `yield` is executed until `next()` is called again
  ```
  function *createIterator(items) {
    for (let i = 0; i < items.length; i++) {
      yield items[i]
    }
  }
  
  const iterator = createIterator([1, 2, 3])
  console.log(iterator.next())     // { value: 1, done: false }
  console.log(iterator.next())     // { value: 2, done: false }
  console.log(iterator.next())     // { value: 3, done: false }  
  console.log(iterator.next())     // { value: undefined, done: true }
  ```
* **Iterable**: an object with `Symbol.Iterator` property
  * All collection objects (arrays, sets, maps) and strings
  * All iterators created by generators are also iterables
  * Can be used with `for-of` loop: calls `next()` method on an iterable until `done` is `true`
  ```
  const values = [1, 2, 3]
  for (let num of values) {
    console.log(num)  // outputs 1, 2, 3
  }
  ```
  * Direct access of iterator of iterables
  ```
  const values = [1, 2, 3]
  const iterator = values[Symbol.Iterator]()
  console.log(iterator.next())     // { value: 1, done: false }
  console.log(iterator.next())     // { value: 2, done: false }
  console.log(iterator.next())     // { value: 3, done: false }
  console.log(iterator.next())     // { value: undefined, done: true }
  ```
  * Non iterable (developer-defined) objects can be iterable by creating `Symbol.Iterator` property containing a generator
  ```
  const myObj = {
    items: [],
    *[Symbol.Iterator]() {
      for (let item of this.items) {
        yield item
      }
    }
  }
  
  myObj.items.push(1)
  myObj.items.push(2)
  myObj.items.push(3)
  
  for (let x of myObj) {
    console.log(x)   // outpust 1, 2, 3
  }
  ```
* Built-in iterators
  * Collection iterators:
    * `entries()`: key-value pairs (two-item array)
    * `values()`: values
    * `keys()`: keys
  * Default iterators for collections (called with `for-of` loop):
    * `values()` for arrays and sets
    * `entries()` for maps
* Spread operator works on all iterables (arrays, sets, maps, even strings)
* You can pass an argument to `next()` method
  * the argument becomes the return value of `yield`
  * when `yield` is assigned, right hand side is executed with `next()`, but left hand side is executed on subsequent `next()`
  
## Chapter 9: JavaScript Classes
* Skip JavaScript Classes for now...

## Chapter 10: Improved Array
* `Array.of()`: creates an array with its arguments regardless of the number of arguments or types
```
const items = Array.of(1, 2)
console.log(items.lengt)     // 2
console.log(items[0])        // 1
console.log(items[1])        // 2
```
* `Array.from()`: converts array-like nonarray objects into actual arrays
```
function doSomething() {
  const array = Array.from(arguments)
  // use array
}
```
* With `Array.from()`, mapping function can be passed to do extra processing on each item before pushed to an array
```
function plusOne() {
  return Array.from(arguments, value => value + 1)
}

const array = plusOne(1, 2, 3, 4, 5)
console.log(array)     // [2, 3, 4, 5, 6]
```
* `find()` and `findIndex()`: takes a callback function and an optional value for `this` inside the callback
```
const numbers = [10, 20, 30, 40]
console.log(numbers.find(item => item > 20))       // 30
console.log(numbers.findIndex(item => item > 20))  // 2
```
* `fill()`: fills one or more array elements with a specific value (it overwrites existing values)
```
const numbers = [1, 2, 3, 4]
numbers.fill(1)
console.log(numbers)    // [1, 1, 1, 1]
```
* with `fill()` optional start and exclusive end index can be included
```
const numbers = [1, 2, 3, 4]
numbers.fill(1, 2)
console.log(numbers)     // [1, 2, 1, 1]
numbers.fill(0, 1, 3)
console.log(numbers)     // [1, 0, 0, 1]
```
* `copyWithin()`: copy a section of elements from the array to another section of the same array
```
const numbers = [1, 2, 3, 4]
numbers.copyWithin(2, 0)
console.log(numbers)          // [1, 2, 1, 2]
```

## Chapter 11: Promises and Asynchronous Programming
* Promise life cycle: `pending`(unsettled) -> `fulfilled` or `rejected`(settled)
* `then()` method: takes two functions as arguments, one for fulfilled case and the other for rejected case
* `catch()` method: syntatic sugar for `then(null, function() {})`, but better handles other error scenarios
* Creating unsettled promises
  * Example:
  ```
  function createPromise() {
    return new Promise((resolve, reject) => {
      // do something asynchronously
      ...
      if (error) {
        reject(error)
        return
      }
      resolve(result)
    })
  }

  createPromise().then(result => {
    console.log(result)
  }).catch(error => {
    console.log(error)
  })
  ```
  * Promise executor(function passed to the promise constructor) is executed immediately when `createPromise()` is called
  * Functions passed to `then()` and `catch()` are executed asynchronously as they are added to the job queue
* Creating settled promises
  * With `Promise.resolve()`: Accepts a single argument and returns a promise in a fulfilled state (No job scheduling)
  ```
  const promise = new Promise.resolve(42)
  promise.then(value => {
    console.log(value)     // 42
  })
  ```
  * With `Promise.reject()`: Accepts a single argument and returns a promise in a rejected state (No job scheduling)
  ```
  const promise = new Promise.reject(42)
  promise.catch(value => {
    console.log(value)     // 42
  })
  ```
* If an error is thrown in the executor, the promise's rejection handler is called
```
const promise = new Promise((resolve, reject) => {
  throw new Error('Something failed!')
})

promise.catch(error => {
  console.log(error)      // 'Something failed!'
})
```
* `then()` and `catch()` returns a new promise, which allows chaining
* When chained, the next promise is resolved only when the previous promise is fulfilled or rejected
* A value can be returned in fulfillment handler or rejection handler to pass values in promise chain
```
const promise = new Promise.resolve(42)
promise.then(value => {
  console.log(value)     // 42
  return value + 1
}).then(value => {
  console.log(value)     // 43
})
```
* When a **promise is returned** in fulfill/reject handler, instead of primitive values, something else happens
* If returned promise is fulfilled, resolved value is returned to next fulfillment handler
* If returned promise is rejected, rjected value is returned to next rejection handler
* Remember, in both `then()` and `catch()`, one of these can be done...
  * return synchronous value
  * return asynchronous promise
  * throw synchronous error
* If something other than a function is passed to `then()` or `catch()`, it will be ignored
* `Promise.all()`: executes multiple promises simulaneously and resolves only when all promises are fulfilled
* `Promise.race()`: instead of waiting for all promises, it resolved as soon as any promise is fulfilled

## Chapter 12: Proxies and the Reflection API
* Skip!!

## Chapter 13: Encapsulating Code with Modules
* Problems with sharing one global scope include naming collisions and security concerns
* What are Modules?
  * JavaScript code automatically runs in strict mode
  * Variables declared in the top level of a module aren't added to the shared global scope
  * Variables exist only within the top level scope of the module
  * Modules export elements like variables or functions
  * Modules may import bindings from other modules
* Exporting
  * Use `export` keyword to expose elements like variables or functions
  ```
  // export variables
  export var color = 'red'
  export let name = 'Sungho'
  export const number = 7
  
  // export functions
  export function sum(num1, num2) {
    return num1 + num2
  }
  
  function subtract(num1, num2) {
    return num1 - num2
  }
  
  export subtract
  ```
  * Anything not exported will be private to the module
  * Anonymous functions cannot be exported
* Importing
  * Use `import` keyword to access exposed elements from a module
  * Importing a single binding:
  ```
  import { sum } from './example.js'
  
  console.log(sum(1, 2))     // 3
  ```
  * Importing multiple bindings:
  ```
  import { sum, subtract, name } from './example.js'
  
  console.log(sum(2, 3))       // 5
  console.log(subtract(3, 2))  // 1
  console.log(name)            // "Sungho"
  ```
  * Importing an entire module:
  ```
  import * as example from './example.js'
  
  console.log(example.sum(1, 2))   // 3
  console.log(example.color)       // "red"
  console.log(example.name)        // "Sungho"
  ```
* Export and Import Renaming
  * Export with different name
  ```
  function sum(num1, num2) {
    return num1 + num2
  }
  export { sum as add }
  ```
  * Import with different name
  ```
  import { add as sum } from './example.js'
  console.log(sum(1, 2))     // 3
  ```
* Default values in modules
  * Default value: A single variable, function or class specified by the `default` keyword
  * You can only set **one** default export per module
  ```
  export default function(num1, num2) {
    return num1 + num2
  }
  ```
  * Note: anonymous function can be exported with `default` keyword
  * Importing default value
  ```
  import sum from './example.js'
  console.log(sum(1, 2))     // 3
  ```
  * Note: No curly braces means loading default value from the module
  * Importing default value with non-default bindings
  ```
  export let name = 'Sungho'
  export default function(num1, num2) {
    return num1 + num2
  }
  ```
  When importing:
  ```
  import sum, { name } from './example.js'
  ```
  Or
  ```
  import { default as sum, name } from './example.js'  // default has to come first!
  ```
