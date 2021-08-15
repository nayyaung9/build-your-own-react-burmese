# Step 3: Fibers

အလုပ်တွေ ဘာပြီးရင် ဘာလုပ်မယ်ဆိုတာ သတ်မှတ်ဖိုအတွက် Fiber tree လိုခေါ်တဲ့ data structure တစ်ခုလိုအပ်ပါတယ်။

JSX Element တစ်ခုချင်းစီအတွက် fiber node တစ်ခု ထားမယ်၊ အဲ့ nodes တွေထဲမှာလည်း သူရဲ့ parent တို၊ child တို၊ နောက် sibling node တွေကတစ်ဆင့် Tree တစ်ခုကို တည်ဆောက်မှာပါ။ အဲ့ဒီလို တစ်ခုနဲ့တစ်ခု အချိတ်အဆက်ရှိနေတဲ့ tree structure ကို Fiber tree လိုခေါ်မှာပါ။

အပေါ်က render function ထဲမှာ အရင်ဆုံး root fiber တစ်ခုကို create လုပ်မယ်။ အဲ့ဒီ process ကို nextUnitOfWork မှာသတ်မှတ်မယ်။ နောက်လုပ်ကမယ့်အလုပ်တွေက performUnitOfWork ထဲမှာဆက်ဖြစ်မယ်။ အဲ့ process မှာ လုပ်ကမယ့် ၃ချက်ရှိတယ်။

- element ကို DOM ထဲထည့်မယ်။
- ခုနက root fiber နဲ့ အချိတ်အဆက်ရှိတာကြောင့် သူ့ရဲ့ child အတွက် fiber တစ်ခု create ဆက်လုပ်မယ်။
- နောက် ဆက်လုပ်ကမယ့် process ကို ထုတ်ပေးမယ်။

ဒီ data structure ( fiber tree ) ရဲ့ အဓိကရည်ရွယ်ချက်က နောက်လုပ်မယ့် အလုပ်ကိုလွယ်လွယ်ကူကူရှာဖိုပါ။ အာ့တာကြောင့် fiber tree ထဲမှာကို first child ရယ်၊ siblings element နဲ့သူရဲ့ parent တိုကို လင့်လုပ်ထားခြင်းဖြစ်ပါတယ်

ဥပမာ၊ parent element အတွက် root fiber create ပြီးရင်၊ အဲ့ဒီ ( parent element ) ထဲမှာ child element ရှိနေသေးရင် အဲ့ child အတွက် fiber တစ်ခုထက်လုပ်မယ်။ အဲ့ process ကို nextUnitOfWork ထဲပိုပေးလိုက်မှာပါ။
တကယ်လို child element မရှိရင် sibling element ကို nextUnitOfWork ထဲ ပိုပေးမယ်

တစ်ကယ်လို fiber ( နောက်ဆုံး element ) မှာ child ရော sibling ရောမရှိတော့ရင် အဲ့ fiber ရဲ့ parent ရဲ့ sibling ကိုပြန်သွားမယ်။ ဥပမာထဲကကြည့်ရင် a နဲ့ h2 fiber ပေါ့။
ဆိုလိုတာက a မှာ child မရှိတော့ parent ဖြစ်တဲ့ h1 ကိုသွားမယ်။ သူရဲ့ sibling ဖြစ်တဲ့ h2 ကိုသွားမယ်။

h2 ကလည်း no child ဆိုတော့ သူရဲ့ parent ဖြစ်တဲ့ div ဆီပြန်သွားမှာပါ။

သူရဲ့သဘောတရားက လင့်လုပ်ထားတဲ့ element ရှိမရှိစစ်မယ်။ ရှိရင် performUnitOfWork မှာအလုပ်လုပ်မယ်။ အဲ့ကနေ နောက်ထပ် လင့်လုပ်ထားတဲ့ element ရှိမရှိစစ်မယ်။ ရှိရင် nextUnitOfWork ထဲပိုမယ်။ နောက်ဆုံး No child, No sibling ဖြစ်လို အောက်ကနေအပေါ်ပြန်တက်သွားပြီး root ကိုပြန်ရောက်သွားမယ်ပေါ့။ အဲ့ခါကျ render process အတွက် အလုပ်လုပ်လိုပြီးသွားပါပြီ။

အခု ကုတ်တင်း ထဲမှာ implement လုပ်ကြမယ်။

```js
...
function createDom(fiber) {

}

function render(element, container) {
  // TODO set next unit of work
}

let nextUnitOfWork = null;
```

render function ထဲမှာ nextUnitOfWork ကို fiber tree ရဲ့ root ထည့်‌ပါမယ်။

```js
...
function render(element, container) {
 nextUnitOfWork = {
   dom: containter,
   props: {
     children: [element]
   }
 }
}

let nextUnitOfWork = null;

```
