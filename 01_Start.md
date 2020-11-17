# Vue3.0 開始

除了改用 `Vue.createApp`之外, 還可以使用`setup()`來讓環境更像在寫Javascript.

```js
const { ref, reactive } = Vue;
const app = {
    setup(){
        let sayHi = ref('Hello World')
        ...
        return {
            sayHi,
            ...
        }
    }
}
Vue.createApp(app).mount('#app')
```