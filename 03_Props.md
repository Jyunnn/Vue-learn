# Vue3.0 props
---
&nbsp;
如果要讓資料傳遞到`Component`裡面,就必須要用`props`

```js
<template>
    <p>這個是Component - Article</p>
    <p>{{props.msg}}</p>  // 4. 插入props
</template>

<script>
export default {
  props: {
    msg: String, // 1.指定一個props並賦予類型
  },
  setup(props) { // 2.帶入參數
    return {
      props,    // 3.回傳參數
    };
  },
};
</script>
```

```js
<template>
  <Article :msg="say" /> // 7.使用並帶入props
  <router-view/>
</template>

<script>
import Article from '@/components/Article.vue'; // 5.匯入 Component -Article
import { ref } from 'vue';

export default {
  setup() {
    const say = ref('Hi');
    return {
      say,
    };
  },
  components: {
    Article, // 6.指定 Component
  },
};
</script>
```
&nbsp;
`props`還可以用一些功能
[官網參考](https://v3.vuejs.org/guide/component-props.html#prop-validation)

比方說你可以用`default`設定預設值
```js
propD: {
  type: Number,
  default: 100
},
```
但是要注意如果預設值要是一個物件,必須用含式回傳
這些官方文件都有提到,建議直接查看
```js
propE: {
      type: Object,
      default: function() {
        return { message: 'hello' }
      }
},
```
