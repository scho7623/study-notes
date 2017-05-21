# Learning JavaScript, 3rd Edition
## Chapter 6. Functions
* Method: When a function is a property of an object
```
const obj = {
  name: 'Sungho',
  greet() {    // greet is a method of obj
    console.log(this.name)
  }
}
```

* `this` keyword: In above example, `this` refers to the object instance `obj`
* `this` is bound according to **how the function is called**, not where the function is defined
```
const greet = obj.greet
greet === obj.greet
greet()   // '' because the function is not directly called on an object, `this` is not bound to anything
```
* Arrow notation (`=>`) function: `this` is bound lexically (bound where the function is defined)
```
const obj = {
  name: 'Sungho',
  greetBackward() {
    const reverseName = () => {
      return reverse(this.name)   // `this` is bound to the obj
    }
    return reverseName()
  }
}
```
* `call`, `apply` and `bind`
  * `call`: call a function with an object bound to `this` as the first argument
  * Rest arguments of `call` are passed to the function as parameters
  * `apply`: identical to `call`, but the second argument is an array of parameters to the function
  * `bind`: returns a function with the first argument as an object bound to `this`, instead of executingn the function

## Chapter 7. Scope
* Global scope
  * The base scope that you are implicitly in when a program starts
  * Anything declared in global scope is available anywhere in your program
* Block scope
  * Block: surrounded by the curly braces
  * `let` and `const` are visible only in the block scope
  * Control flows such as `if` and `for` has its own block scope
* Closures
  * A function declared inside another function, which has access to variables in the outer function scope
  ```
  let globalFunc
  {
    let blockVar = 'a'    // block scoped variable
    globalFunc = function() {
      console.log(blockVar)
    }
  }
  globalFunc()    // logs 'a'
  ```
* Immediately Invoked Function Expressions (IIFE)
  * Declares a function and executes it immediately
  * It creates its own function scope and able to pass something out of this scope
  ```
  const fn = (function() {
    let count = 0
    return function () {
      console.log(`Executed ${++count} times!`)
    }
  })()
  ```
* Function scope and Hoisting
  * Unlike `let` and `const`, `var` is declared in a function scope
  * When `var` is declared, it is available anywhere in the function scope even **before** it's declared
  * This is because `var` declaration is hoisted to the top (only declaration, not assignment)
  ```
  x  // undefined                         var x
  var x = 3                 =>            x
  x  // 3                                 x = 3
                                          x
  ```
* Function hoisting
  * Just like `var`, function declarations are hoisted to the top of their scope, and available anywhere
  ```
  f()   // 'f'
  function f() {
    console.log('f')
  }
  ```
  * Note function expressions assigned to variables are not hoisted
  ```
  f()   // TypeError: f is not a function
  let f = function () {
    console.log('f')
  }
  ```
* The Temporal Dead Zone (TDZ)
  * Variables declared with `let` is not available until it is declared
  ```
  f   // Error! TDZ!
  let f = 3
  ```
* Strict mode
  * `'Use strict'` to enable strict mode
  * When strict mode is enabled, implicit globals are prevented
  
## Chapter 8. Arrays
* `push`: add an element to the end of array (in place)
* `pop`: remove an element at the end of array and return the element (in place)
* `unshift`: add an element to the beginning of array (in place)
* `shift`: remove an element at the beginning of array and return the element (in place)
* `concat`: add multiple elements to the end of array and return the copy (original array not modified)
  * if an array is passed, it breaks apart elements and append them to the target array
* `slice(start, end)`: get subarray starting from start index and end at end index (exclusive)
  * if negative index: count from behind (ex: -1 -> the last element, -2 -> the second last element)
  * subarray is returned and the original array is unmodified
  ```
  const arr = [1, 2, 3, 4, 5]
  arr.slice(3)                 // returns [4, 5]; arr unmodified
  arr.slice(2, 4)              // returns [3, 4]; arr unmodified
  arr.slice(-2)                // returns [4, 5]; arr unmodified
  arr.slice(1, -2)             // returns [2, 3]; arr unmodified
  arr.slice(-2, -1)            // returns [4]; arr unmodified
  ```
* `splice(start, elementsToRemove, newElement, ...)`
  * First argument: start index to modify
  * Second argument: number of elements to remove
  * Remaining arguments: elements to be added
  ```
  const arr = [1, 5, 7]
  arr.splice(1, 0, 2, 3, 4)    // returns []; arr is now [1, 2, 3, 4, 5, 7]
  arr.splice(5, 0, 6)          // returns []; arr is now [1, 2, 3, 4, 5, 6, 7]
  arr.splice(1, 2)             // returns [2, 3]; arr is now [1, 4, 5, 6, 7]
  arr.splice(2, 1, 'a', 'b')   // returns [5]; arr is now [1, 4, 'a', 'b', 6, 7]
  ```
* `copyWithin`: copy elements from an array and overwrite target elements
* `fill`: fill the array with given value
* `reverse`: reverse the array
* `sort`: sort the array with optional sort function
