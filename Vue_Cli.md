# Vue Cli
---

這次要注意的是
1. `<template>`可以不用一個`<div>`包起來

2. 如果要用`ref`之類的函數,需要用
    ```js
    import { ref } from 'vue';.
    export default {
      ...
    }
    ```
3. 如果有全域的CSS,可以在`assets`內新增一個css資料夾
   並在`main.js`內`import "@/assets/css/XXX.css"`

4. 獲得`event`的方法 :
   在`v-on`後面的函式參數新增`$event` (如果有其他參數, `$event`一定要放在最後一個)
    ```html
    <button @click="isopen($event)">OPEN</button>
    ```
    就可以只用`event`事件了
    ```js
    const isopen = function (e) {
      console.log(e);
    };
    ```
5. 取得DOM元素方式 :
   在DOM元素新增`ref`屬性並命名, 並在`setup()`內新增與剛剛元素內屬性名稱相同的變數, 賦予`ref(null);`
   就可以取得指定的DOM元素了
   *記得 `import { onMounted, ref } from 'vue';`*
    ```html
    <input ref="nameInput" type="text">
    ```
    ```js
    setup() {
        const nameInput = ref(null);
        onMounted(() => {
          nameInput.value.focus(); // 讓焦點直接用在 這個input上
        });
        return {
          nameInput,
        };
    }
    ```
6. 元件命名方式 :
   大寫開頭,如果是駝峰式命名,使用`<slot>`時要記得改用kebab case (改用小寫,並用'-'連結字串)

7. Router History modes :
   在使用`npm run build`後, 使用LiveServer首頁的部分可能會有問題, 因為`router`設定首頁是用`/`, LiveServer會把首頁導向index.html
   此時可以暫時嘗試切換為`createWebHistory`, 不過不建議切換, 可以修改後端設定
   [參考文件](https://next.router.vuejs.org/guide/essentials/history-mode.html#hash-mode)
