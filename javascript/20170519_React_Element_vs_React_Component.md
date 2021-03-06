# React Element vs React Component
From [this](https://medium.freecodecamp.com/react-elements-vs-react-components-fdc776705880#.p6xc4sawf) medium post.

Also check [this](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html) from Dan Abramov

## React Element
* Plain JavaScript object representation of DOM node or another React element
* Why represent DOM node as an object?
  * React can easily create and destroy with minimun overhead
  * React can analyze the object and actual DOM, then update the actual DOM only where change occurred!
```
const element = React.createElement( 
  'div', 
  {id: 'login-btn'}, 
  'Login'
)
```
This returns following JavaScript object:
```
{
  type: 'div',
  props: {
    children: 'Login',
    id: 'login-btn'
  }
}
```
This is a representation of following DOM node:
```
<div id="login-btn">Login</div>
```

## React Component
* Function or class which takes props as an input and returns a React element as an output
```
function Button ({ addFriend }) { 
  return React.createElement( 
    'button', 
    {onClick: addFriend}, 
    'Add Friend' 
  )
}
```
With JSX:
```
function Button ({ addFriend }) { 
  return ( 
    <button onClick={addFriend}>Add Friend</button> 
  )
}
```
