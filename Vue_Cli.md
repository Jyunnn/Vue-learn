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
