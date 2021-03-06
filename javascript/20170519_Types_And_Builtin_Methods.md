# JavaScript Types and Builtin Methods
## Primitive types
* Number
* String
* Boolean
* Null
* Undefined
* Symbol

## Builtin methods to keep in mind...
* `Array.slice` vs `Array.splice`
  * `Array.slice` **does not** alter original array
  ```
  const original = ['red', 'blue', 'green']
  const sliced = original.slice(0, 2)      // copy from index 0 to index 2 (but end index not inclusive)
  console.log(original)                    // "['red', 'blue', 'green']"
  console.log(sliced)                      // "['red', 'blue']"
  ```
  * `Array.splice` **does** alter original array
  ```
  const original = ['red', 'blue', 'green']
  const spliced = original.splice(0, 2)    // remove 2 elements from index 0
  console.log(original)                    // "['green']"
  console.log(spliced)                     // "['red', 'blue']"
  ```
  * Insertion is also possible with `Array.splice`
  ```
  const original = ['red', 'blue', 'green']
  const spliced = original.splice(1, 0, 'black', 'white')    // insert two elements before index 1
  console.log(original)                                      // "['red', 'black', 'white', 'blue', 'green']"
  console.log(spliced)                                       // "[]"
  ```
* `String.slice(start, end)`: extract string from start index to end index(end not included)
* `String.substring(start, end)`: extract string from start index to end index(end not included)
* `String.substr(start, length)`: extract string from start index for `length` number of characters
* `Object.assign()`
* `Object.keys()`
