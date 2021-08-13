# Step 2: The render function

React Dom ရဲ့ render function ကို ကိုယ်ပိုင် version နဲ့ ရေးပါမယ်။

အခုအချိန်မှာတော့ DOM ထဲ element adding လုပ်တာပဲ အာရုံစိုက်ပါ့မယ်။ နောက်ပိုင်း updating နဲ့ deleting အပိုင်းကို ဆက်လုပ်ပါ့မယ်။

```js
...

function render(element, container) {
  // TODO create dom nodes
}

const Didact = {
  ...
  render,
}

...

Didact.render(element, container)

```

ရှေ့ခန်းက element object ကိုမှတ်မိဦးမှာပါ။ အာ့ထဲက type ကို အခု DOM node အနေနဲ့ create လုပ်မှာပါ။

```js
...

function render(element, container) {
  const dom = document.createElement(element.type);

  container.appendChild(dom);
}

...

```

ဒီနေရာမှာ မရောပါနဲ့၊ ကျွန်တော်တိုအပေါ်မှာလုပ်ခဲ့တဲ့ Didact's createElement function ကနေ ထွက်လာတဲ့ type ကို အခု DOM ထဲထည့်ဖို document.createElement function ကိုသုံးထားတာပါ။
ပြီးနောက် container ထဲ node ကိုပြန်ထည့်လိုက်ပါတယ်။

children ထဲမှာလည်း element object လို type and props ပါပါတယ်။ ရှေ့အခန်းမှာပြောခဲ့သလို children သည် TEXT_ELEMENT လည်းဖြစ်နိုင်သလို arrays လည်းဖြစ်နိုင်ပါတယ်။ ဆိုတော့...

```js
function render(element, container) {
  const dom = document.createElement(element.type);

  +element.props.children.forEach((child) => render(child, dom));

  container.appendChild(dom);
}
```

ပြီးနောက် text element ကို handle လုပ်ရပါမယ်။ တကယ်လို element type သည် TEXT_ELEMENT ဖြစ်တယ်ဆိုရင် text node တစ်ခု create လုပ်ပေးပြီး မဟုတ်ရင်တော့ ပုံမှန် node တစ်ခုသာ ဖန်တီးပေးရပါမယ်။

```js
function render(element, container) {
  const dom =
    element.type == "TEXT_ELEMENT"
      ? document.createTextNode("")
      : document.createElement(element.type);

  element.props.children.forEach((child) => render(child, dom));

  container.appendChild(dom);
}
```

နောက်ဆုံးတစ်ခုကတော့ element props တွေကို node ထဲ assign လုပ်ပေးရမှာပါ။

```js
...

const isProperty = key => key !== 'children';

Object.keys(element.props)
.filter(isProperty)
.forEach(name => {
  dom[name] = element.props[name]
})

...

```
အခုဆိုရင်တော့ ကျွန်တော်တိုကိုယ်ပိုင် JSX render လုပ်နိုင်မယ့် Library တစ်ခုတော့ရခဲ့ပါပြီ။

Try ကြည့်လိုရပါတယ်ဗျ။