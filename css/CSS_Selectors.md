# CSS Selectors
## `~` vs `+`
* `a ~ b`: select all `b` elements present after `a` element
* Example: [Codepen](http://codepen.io/anon/pen/wgbeqy)
```
<style>
p ~ ul {
  background: #ff0000;
}
</style>
<div>
  <ul>
    <li>This is a list item</li>       --> Won't have red background!
  </ul>
  <p>This is a paragraph</p>
  <span>Something else in between</span>
  <ul>
    <li>This is a list item</li>       --> Will have red background!
  </ul>
</div>
```
* `a + b`: select `b` element which is present immediately after `a` element
* Example: [Codepen](http://codepen.io/anon/pen/ZLNyXZ)
```
<style>
p + ul {
  background: #ff0000;
}
</style>
<div>
  <p>This is a paragraph</p>
  <ul>
    <li>This is a list item</li>       --> Will have red background!
  </ul>
  <p>This is a paragraph</p>
  <span>Something else in between</span>
  <ul>
    <li>This is a list item</li>       --> Won't have red background!
  </ul>
</div>
```
