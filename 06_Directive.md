# Vue3.0 directive
---
&nbsp;
除了官方提供的`v-bind`和`v-show`...等,還可以自己自訂義這些指令
[官方文件](https://v3.vuejs.org/guide/custom-directive.html#intro)
先到main.js文件,會因為一開始設定有選擇的項目有所不同
先把`createApp(App)`賦予給一個變數, 在使用`directive`方法

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';

const app = createApp(App);

app.directive('focus', {

});

app.use(store).use(router).mount('#app');
```
這麼一來就能在組件上使用`v-focus`
DOM元素會因為渲染完成後,把元素回傳到`el`參數,
這時候在用`focus()`就等同於`document.getElementById().focus()`

---

如果想要將這個自訂義指令帶入值,例如`v-qty="3"`,可以在新增一個參數
```html
<template>
  <p v-qty="3"></p>
</template>
```
```js
app.directive('qty', {
  mounted(el, binding) {
    const element = el;
    element.innerHTML = binding.value;
  },
});
```

除了`mounted`還能用其他生命週期,
有用`v-model`綁定`<input>`的話,
可以用`updata()`
否則值不再更新(生命週期原因)

```js
app.directive('qty', {
  mounted(el, binding) {
    const element = el;
    element.innerHTML = binding.value;
  },
  updated(el, binding) {
    const element = el;
    element.innerHTML = binding.value;
  },
});
