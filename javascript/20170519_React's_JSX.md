# React's JSX
[Here](https://medium.com/@housecor/react-s-jsx-the-other-side-of-the-coin-2ace7ab62b98#.8baghte6j) is a medium post explaining about React
's JSX. It explains how working with HTML and JavaScript has evolved from "Separation of conerns" to "JSX".

## Phase 1: Unobtrusive JavaScript
* Separation of concerns!
* Pure HTML, Pure JavaScript!
```
// HTML
<a class="btn">Click me!</a>

// JS
$('a.btn').click(function () {
  alert('Link clicked!');
});
```
* Problem: How do I tell these two are interconnected?
* To modify a line of HTML, every line of JS has to be checked to make sure selectors aren't broken
* Strictly seperating HTML and JS => Harder to maintain and debug! :(

# Phase 2: Two-way binding
* Like Angular, Ember, and Knockout
* We realized intermingling HTML and JavaScript might be okay!
* But why put JavaScript into HTML???

# Phase 3: JSX!
* HTML and JavaScript belong together
* React puts HTML into JavaScript (unlike Angular, Ember, and Knockout)
* Here are some benefits:
  * Compile time error: any typo, missing closing tags will generate compile error
  * Can leverage full power of JavaScript, no proprietary conditionals or loops!
