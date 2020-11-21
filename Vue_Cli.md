# Vue Cli
---

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
3. 