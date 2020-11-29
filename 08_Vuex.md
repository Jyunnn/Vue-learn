# Vue3.0 vuex
---
&nbsp;
vuex可以在cli安裝前選擇是否要安裝,
安裝後會在`store`內的`index.js`,
可以將資料放進文件內

使用的方式有兩種,
2.0的方法
```js
$store.state.value
// 如果是在 export default 內使用, 必須加 this
```

3.0的方法
```js
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore();
    return {
        count: computed(() => store.state.count),
    };
  },
};
```
