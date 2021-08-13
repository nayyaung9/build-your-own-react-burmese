# Step 0: Reviews

ဒီ Code တွေကိုနားလည်ဖို React ရဲ့အခြေခံအလုပ်လုပ်ပုံ၊ JSX နဲ့ DOM elements တွေအလုပ်လုပ်ပုံကိုသိရှိထားဖိုလိုပါတယ်။

## JSX ဆိုတာဘာလဲ

JSX ရဲ့အရှည်ကောက်က JavaScript XML ပါ။ အလွယ်ပြောရရင် JavaScript codes တွေ ထဲ HTML tags တွေထည့်ရေးတဲ့အခါမှာသုံးပါတယ်။

ကျွန်တော်တိုကိုယ်ပိုင် React App တစ်ခုကို code သုံးကြောင်းနဲ့စရေးကြည့်မယ်။ ပထမဆုံး React Element တစ်ခုကို define လုပ်မယ်။ ပြီးတော့ node တစ်ခုကို DOM ထဲက create လုပ်မယ်။
ခုနက React Element ကို container ထဲ render လုပ်လိုက်မယ်

```javascript
const element = <h1 title="foo">Hello</h1>;
const container = document.getElementById("root");
ReactDOM.render(element, container);
```

ပထမဆုံး အကြောင်းမှာ element ဆိုတဲ့ variable ရှိတယ်။ သူ့ကို JSX နဲ့ define လုပ်ထားတယ်။

Browsers တွေက Pure JS တစ်ခုပဲနားလည်တာပါ။ ခုနက ရေးခဲ့တဲ့ JSX ကို JS ပြောင်းဖိုဆို Babel ဆိုတဲ့ Tool ကိုအသုံးပြုရပါတယ်။ Transformation က ရိုးရိုးရှင်းရှင်းလေးပါ။ createElement ဆိုတဲ့ function ထဲမှာ tag name ထည့်မယ်၊ props နဲ့ children ကိုတော့ parameters အနေနဲ့ pass လုပ်ပေးလိုက်တယ်။

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

React.createElement က သူ့ arguments ကနေ Object တွေပြန်ထုတ်ပေးတယ်။ တကယ့် React Source Codes ထဲတော့ validation တွေတော့ရှိတယ်။ ဒီမှာတော့ ဒီလောက်ဆို အဆင်ပြေပါတယ်။

> နောက် createElement function from scratch ကနေရေးပြမှာပါ။

```js
const element = {
  type: "h1",
  props: {
    title: "foo",
    children: "Hello",
  },
};
```

နောက် ရေးမယ့် createElement function ကနေ အခုလို format နဲ့ output ပြန်လည်ရရှိပါမယ်။
properties ၂ခုရှိမယ် ( type နဲ့ props ) ပါ။ တကယ့် source code မှာတော့ ဒီထက်ပိုများပေမယ့် ဒီ tutorial မှာတော့ ဒီ ၂ခုပဲ Focus ကြပါမယ်။

type က typeof string ဖြစ်ပြီးတော့ ကျွန်တော်တို create လုပ်ချင်တဲ့ HTML tag တစ်ခုကို ထည့်ပေးရုံပါ။ အာ့နေရာမှာ function လည်းဖြစ်နိုင်ပါတယ်။ နောက် Chapter တွေမှာ အာ့အကြောင်းဆက်ပါ့မယ်။

နောက် object တစ်ခုက props ဖြစ်ပြီးတော့ သူ့က JSX attributes တွေကနေ ရရှိပြီး keys and values အတွဲအနေနဲ့ရှိပါတယ်။ ပြီးတော့ props မှာ special property ဖြစ်တဲ့ children တစ်ခုလည်းရှိပါသေးတယ်။

အခု ဒီ code မှာတော့ children က typeof string ပေါ့၊ ဒါပေမယ့် children ဆိုတာ တစ်ခုထက်ပိုလို အမြဲ array အနေနဲ့ရှိပါတယ်။ ဒါကြောင့် element တွေက trees structure ဖြစ်နေရတာပါ။

---

```js
ReactDOM.render(element, container);
```

နောက်ဆက်ကြည့်ရမှာက ReactDOM ရဲ့ render function ပါ။ render လုပ်တာက React Components တွေကို DOM ထဲ append လုပ်ပြီး changes လုပ်တယ်လိုမှတ်ယူနိုင်ပါတယ်။ ဘယ်လိုလုပ်လဲဆက်ကြည့်ရအောင်

```js
const node = document.createElement(element.type);
node["title"] = element.props.title;
```

အခုကျွန်တော်တို node ( DOM element ) တစ်ခုကို create လုပ်မယ်၊ ဘာ node လဲဆိုတော့ ခုနက ရေးခဲ့တဲ့ element ထဲက type property ပါ။

ပြီးတာနဲ့ အာ့ element ရဲ့ props တွေကို ဒီ node ထဲ assign လုပ်မယ်။

> အသုံးနှုန်းသတိပြုပေးပါ၊ ကျွန်တော် React Element ကို "element" လိုသုံးပြီး DOM Element ကိုတော့ "node" လိုသုံးပါမယ်။

```js
const text = document.createTextNode("");
text["nodeValue"] = element.props.children;
```

ပြီးတဲ့နောက် props ထဲပါလာတဲ့ children node တစ်ခု create လုပ်ပါမယ်။ ဒီ ဥပမာမှာတော့ children က typeof string အနေနဲ့ပါလာတာမလို text node ကိုသာ create လုပ်ပါမယ်။

```js
const container = document.getElementById("root")
​
node.appendChild(text)
container.appendChild(node)
```

နောက်ဆုံးအနေနဲ့ textNode ကို h1 ထဲ append လုပ်မယ်၊ အာ့ h1 ကို container ထဲ
ထပ်ထည့်ပါမယ်။

```js
const element = {
  type: "h1",
  props: {
    title: "foo",
    children: "Hello",
  },
}
​
const container = document.getElementById("root")
​
const node = document.createElement(element.type)
node["title"] = element.props.title
​
const text = document.createTextNode("")
text["nodeValue"] = element.props.children
​
node.appendChild(text)
container.appendChild(node)
```

React ကိုမသုံးပဲနဲ့ သူ့လိုမျိုး အလုပ်လုပ်နိုင်မယ့် app လေးတစ်ခုတော့ရပြီဗျ။

[Step I: The createElement Function](https://github.com/nayyaung9/build-your-own-react-burmese/blob/main/1-step.md)
