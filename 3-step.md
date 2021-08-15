# Step 3: Concurrent Mode

ဒီအပိုင်းမှာတော့ ကုတ်အသစ်တွေမထည့်ခင် refactoring လုပ်ဖိုလိုတယ်ဗျ။ ဒီ recursive call မှာ ပြဿနာနည်းနည်းရှိတယ်ဗျ။

```js
function render(element, container) {
  const dom = element.type ...

  element.props.children.forEach(child =>
    render(child, dom)
  )
}
```

rendering တစ်ကြိမ်စလုပ်တာနဲ့ element tree မပြီးတဲ့အထိ သူက ဆက်လုပ်နေမှာ၊ တကယ်လို element tree က ကြီးနေတယ်ဆိုရင် main thread ( browser ကနေ user events နဲ့ paints တွေကို process လုပ်တဲ့အပိုင်း ) ကို block သွားလိမ့်မယ်။ပြီးတဲ့နောက် browser ကနေ user input ကို handle လုပ်တာတွေ၊ animation တွေက ဒီ render process ကြီးပြီးတဲ့အထိကို စောင့်နေရတာပေါ့။

```js
let nextUnitOfWork = null
​
function workLoop(deadline) {
  let shouldYield = false
  while (nextUnitOfWork && !shouldYield) {
    nextUnitOfWork = performUnitOfWork(
      nextUnitOfWork
    )
    shouldYield = deadline.timeRemaining() < 1
  }
  requestIdleCallback(workLoop)
}
​
requestIdleCallback(workLoop)
​
function performUnitOfWork(nextUnitOfWork) {
  // TODO
}
```

ဆိုတော့ ကျွန်တော်တိုက ဒီ render process ကို small units လေးတွေအဖြစ် ပြောင်းလဲပြီး အလုပ်လုပ်ခိုင်းမယ်။ unit တစ်ခုပြီးတိုင်း browser ကိုလုပ်စရာရှိတာ ဆက်လုပ်ခိုင်းပြီး render process နဲ့ တစ်လှည့်စီ ပြန် run ပါမယ်။

requestIdleCallback ကိုသုံးပြီး loop လုပ်မယ်။ requestIdleCallback ကို setTineout function လိုမှတ်ယူနိုင်ပါတယ်။ main thread က လုပ်စရာဘာမှမရှိတဲ့အချိန် render process ကို ပြန်ခေါ်ပြီး run ပါမယ်။

React ကတော့ requestIdleCallback ကိုမသုံးတော့ပါဘူး၊ သူတိုက schedular package ကိုသုံးထားပါတယ်။ သဘောတရားကတော့ အတူတူပါပဲ။

requestIdleCallback function ကနေ deadline ဆိုတဲ့ parameter တစ်ခုရရှိပါမယ်။ အဲ့တာကိုသုံးပြီးတော့ browser main thread ပြန်အလုပ်လုပ်ဖို အချိန်ဘယ်လောက်ရှိမလဲဆိုတာ တွက်နိုင်ပါတယ်။

အပေါ်က ကုတ်တွေ ( loop ) တွေစသုံးနိုင်ဖို ပထမဆုံး ကျွန်တော်တိုကိုယ်တိုင် ဘယ်အလုပ်ကိုအရင်စလုပ်ရမယ်ဆိုတာ သတ်မှတ်ပေးရပါမယ်။ အာ့အတွက် performUnitOfWork function ရေးမယ်။ သူက သတ်မှတ်ထားတဲ့အလုပ်တွေလည်းလုပ်ပြီး နောက် ထပ် ဆက်လုပ်ရမယ့်အလုပ်တွေပါ returns ပြန်ပေးပါလိမ့်မယ်။