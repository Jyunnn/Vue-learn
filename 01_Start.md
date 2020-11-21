# Vue3.0 start
---
&nbsp;
除了改用 `Vue.createApp`之外, 還可以使用`setup()`來讓環境更像在寫Javascript.
```js
const { ref, reactive } = Vue; // 等等會提到
const app = {
    setup(){
        let sayHi = ref('Hello World')
        ... // 寫在這邊
        return {
            sayHi,
            // 這邊要將上面的變數回傳,每次都會忘記
            ...
        }
    }
}

Vue.createApp(app).mount('#app')
```

### ref
可以接受所有型態的資料,呼叫值的時候必須要用`.value`
```js
    let imgIndex = ref(0);
    console.log(imgIndex.value) // 0
```
但是要注意如果是用在`html`標籤內,是不用加`value`的
```html
    <div id="app">
        {{ imgIndex }}
    </div>

    <script>
    ... // 忽略Vue頭尾
        let imgIndex = ref(0);
        return { imgIndex }
    </script>
```

雖然可以用`array`和`object`,但是使用`watch`時不會對`array`和`object`內部的屬性變動監聽

### reacctive
只能接受`array`和`object`,呼叫的時候直接使用即可,可以做深層監聽
```js
    let imgs = reactive({data:[]});
    console.log(imgs.data.length) // 0 ,直接呼叫 imgs.data
```


&nbsp;
&nbsp;
