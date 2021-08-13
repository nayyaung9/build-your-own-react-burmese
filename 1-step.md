# Step 1: The createElement function

ဒီ Step မှာ ကျွန်တော်တိုကိုယ်တိုင် React own version ရေးပါ့မယ်။

ပထမဆုံးရေးမှာက createElement function ပေါ့၊ ပြီးတော့ JSX ကနေ JS ကို transform လုပ်ပါမယ်။

```js
const element = {
  type: "h1",
  props: {
    title: "foo",
    children: [],
  },
};
```

ခုနက step မှာတွေ့ခဲ့တဲ့ element မှာ type နဲ့ props ရှိတယ်။ ကျွန်တော်တိုလုပ်ရမှာတစ်ခုက အဲ့ကနေ Object ထုတ်ရုံပါပဲ။

props အတွက် spread operator ကိုသုံးပြီးတော့ children အတွက် rest parameter ကိုသုံးပါမယ်။ children က အမြဲတမ်း typeof array မလိုပါ။

```js
function createElement(type, props, ...children) {
  return {
    type,
    props: {
      ...props;
      children,
    },
  }
}
```

## Examples

```js
createElement("div")

{
  type: "div",
  props: { children: [] }
}
```

```js
createElement("div", null, a)

{
  type: "div",
  props: { children: [a] }
}
```

```js
createElement("div", null, a, b)

{
  type: "div",
  props: { children: [a, b] }
}
```

Children array မှာက အရင် Step တုန်းကလို primitive values တွေဖြစ်တဲ့ strings or numbers တွေလည်းပါလာနိုင်တယ်။ အချိန်တိုင်းတော့ object ဖြစ်မှာမဟုတ်တဲ့အတွက် သူ့အတွက် special type တစ်ခု create လုပ်ပေးဖိုလိုတယ်။

```js
function createElement(type, props, ...children) {
  return {
    type,
    props: {
      ...props;
      children: children.map(child =>
      typeof child === "object"
      ? child
      : createTextElement(child)
      ),
    },
  }
}

function createTextElement(text) {
  return {
    type: 'TEXT_ELEMENT',
    props: {
      nodeValue: text,
      children: []
    }
  }
}
```

> တကယ့် React ကတော့ primitive values နဲ့ children မပါတဲ့ empty arrays တွေကို wrap မလုပ်ပါဘူး။ ဒီမှာ ကျွန်တော်တိုလုပ်ထားပါတယ်။ ဘာလိုလဲဆိုတော့ ကျွန်တော်တိုက performance ကောင်းတဲ့ ကုတ်ထက် ရိုးရှင်းတဲ့ကုတ်ကို ဦးစားပေးလေ့လာမှာမလိုပါ။

ကျွန်တော်တိုကိုယ်ပိုင် နာမည်ပေးလိုက်ပါမယ်။ Didact လိုပေးထားပါတယ်။

```js
function createElement() { ... }

function createTextElement() { ... }

const Didact = {
  createElement,
}

const element = Didact.createElement(
  "div",
  { id: "foo" },
  Didact.createElement("a", null, "bar"),
  Didact.createElement("b")
)
const container = document.getElementById("root")
ReactDOM.render(element, container)

```

သိုပေမယ့် ကျွန်တော်တိုက JSX ကိုသုံးချင်သေးတယ်။ React ရဲ့ createElement ကိုမသုံးပဲ ကျွန်တော်တိုရဲ့ Didact's createElement function ကိုပဲသုံးစေချင်တယ်ဆိုရင် ဘယ်လိုလုပ်ရမလဲ?

```js
...

/** @jsx Didact.createElement */
const element = (
  <div id="foo">
    <a>bar</a>
    <b />
 </div>
 ...
)
```
ဒီ comment ထည့်ပေးရုံပါ။ အာ့အခါကျရင် babel က JSX ကို transpiles လုပ်တဲ့အခါ ကျွန်တော်တို define လုပ်ထားတဲ့ function ကိုသာသုံးသွားမှာပါ။

[Step 2: The render Function](https://github.com/nayyaung9/build-your-own-react-burmese/blob/main/2-step.md)
