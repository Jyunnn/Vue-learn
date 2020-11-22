# Vue3.0 transition
---
&nbsp;
Vue有內建轉場動畫效果`transition`
[官方文件](https://v3.vuejs.org/guide/transitions-overview.html#class-based-animations-transitions)

## 使用方式
&nbsp;
搭配`v-if`或是`v-show`使用,
將想要用`opacity`的標籤用 `<transition>`包起來, 並用`name`屬性命名.
```html
<button @click="isopen">OPEN</button>
<transition name="fade">
    <p v-if="open">IS OPEN!!!!</p>
</transition>
```
```js
setup() {
    const open = ref(false);
    const isopen = function () {
      open.value = !open.value;
    };
    return { open,isopen };
}
```
![vue](https://v3.vuejs.org/images/transitions.svg)
用這張官方提供的圖片做為參考

接下來參考官方提供的圖片,每個流程都會有個 css 名稱
- v-enter-from
- v-enter-active
- v-enter-to
- v-leave-from
- v-leave-active
- v-leave-to
開頭的`v`就是在用`<transition>`標籤時,所命名的`name`值
```css
.fade-enter-active,
.fade-leave-active {
transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
opacity: 0;
}
```

這樣就有淡入淡出的效果了
&nbsp;
---
&nbsp;
也可以用在 CSS Animations
```css
.fade-enter-active {
    animation: move 1s;
}
.fade-leave-active {
    animation: move 1s reverse;
}
@keyframes move {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.25);
    }
    100% {
      transform: scale(1);
    }
}
```
