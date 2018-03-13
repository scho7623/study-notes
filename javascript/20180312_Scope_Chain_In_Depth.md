# Scope chain in depth

## Definition
Scope chain is a list of variable objects which is used for identifier resolution. It is created when function is called and enters into an execution context. It consists of current activation object and `[[Scope]]` property, which is a list of variable objects from parent context. This `[[Scope]]` property is saved to the function at its creation.

## Function life cycle
* Function creation
  * `[[Scope]]` property is created and saved to the function at creation
* Function activation
  * Remember the fact that `Scope` property of activation object is
  ```
  Scope = AO + [[Scope]]
  ```
  * Current activation object is in front of rest of scope chain
  * Identifier resolution: A process of determining which variable object in the scope chain, the identifier belongs to
  * Therefore, local variables of a context have higher priority than variables from parent contexts

## Important features related to scope
* Closure
  * `[[Scope]]` is saved at function creation
  * In another words, `[[Scope]]` contains _lexical environment_ in which the function is _created_
* Two-dimentional scope chain lookup
  * When it comes to identifier resolution, prototype of variable object is also considered
  * Note that activation object does not have prototype
* Affecting scope chain during code execution
  * `with` statement and `catch` clause
  * Add an object in front of the scope chain
