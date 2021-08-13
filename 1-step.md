# Let's write our own createElement

```js
const element = (
  <div id="foo">
    <a>bar</a>
    <b />
  </div>
)
const container = document.getElementById("root")
ReactDOM.render(element, container)
```

 This time we’ll replace React code with our own version of React.

We’ll start by writing our own createElement.

Let’s transform the JSX to JS so we can see the createElement calls.
