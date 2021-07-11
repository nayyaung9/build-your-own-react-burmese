# Reviews

```javascript
const element = <h1 title="foo">Hello</h1>
const container = document.getElementById("root")
ReactDOM.render(element, container)
```

JSX is transformed to JS by build tools like Babel. The transformation is usually simple: replace the code inside the tags with a call to createElement, passing the tag name, the props and the children as parameters.

```javascript
const element = React.createElement(
  "h1",
  { title: "foo" },
  "Hello"
)
​
const container = document.getElementById("root")
ReactDOM.render(element, container)
```

React.createElement creates an object from its arguments. Besides some validations, that’s all it does. So we can safely replace the function call with its output.