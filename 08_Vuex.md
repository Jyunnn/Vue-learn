# Vue3.0 vuex
---
&nbsp;
vuex可以在cli安裝前選擇是否要安裝,
安裝後會在`store`內的`index.js`,
可以將資料放進文件內

`store\index.js`裡面應該是長這樣:
```js
import { createStore } from 'vuex';

export default createStore({
  state: {...},
  mutations: {...},
  actions: {...},
  modules: {...},
});

```
## state

資料都會放在`state`內

在元件內使用的方式有兩種,
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
        count: computed(() => store.state.isRoot),
    };
  },
};
```

# Vuex Store
接下來是store資料夾內的說明

## getters
不建議直接存取`state`的資料,
可以改用`getters`,
但預設一開始不會幫你建立`getters`
必須自己建立
剛剛使用範例是用`store.state.isRoot`
這樣就必須改用`$store.getters.isRoot`
```js
import { createStore } from 'vuex';

export default createStore({
  state: {
    isRoot: false,
  },
  getters: {
    isRoot: (state) => state.isRoot,
  },
});
```


## mutations

改變`state`資料的方法,配合`actions`使用
`mutations`放入函式,該函式第一個參數`state`就是指向放置資料的那個`state`
函式內直接寫更改,但是這樣還是不能用

第二個參數可以從`actions`接收資料,會在等等解釋

```js
state: {
    isOpen: false,
},
mutations: {
    handleStateIsOpen(state) {
      state.isOpen = !state.isOpen;
    },
},
```
## actions
這時候就要使用`actions`
`actions`內一樣是放函式,函式名稱可以跟剛剛在`mutations`內的那個不同,
但是這裡的名稱會在組件上使用
參數為`context`
函式內調用`context`的函式方法`commit`
`commit`裡面第一個參數為剛剛在`mutations`內要使用的函式名稱
第二個參數可以將資料傳回到`mutations`
範例中給常數 *bool* 賦予 *isOpen* 的相反值
再將它置入於`commit`的第二個參數

在從`mutations`內新增的函式第二個參數 *fromActions* 接收

```js
export default createStore({
  state: {
    isOpen: false,
  },
  mutations: {
    handleStateIsOpen(state, fromActions) {
      state.isOpen = fromActions;
    },
  },
  actions: {
    handleIsOpen(context) {
      const bool = !context.state.isOpen;
      context.commit('handleStateIsOpen', bool);
    },
  },
  modules: {},
  getters: {
    isOpen: (state) => state.isOpen,
  },
});
```

回到要使用這個方法的組件,
要調用剛剛在`actions`新增的方法
一樣先`import { useStore } from 'vuex'`
在使用參數包裝` const store = useStore();`
使用`store.dispatch('actionsName');`
*actionsName* 就是剛剛在`actions`新增的方法名稱

```js
import { computed } from 'vue';
import { useStore } from 'vuex';

export default {
  setup() {
    const store = useStore();
    const showMenu = () => {
      store.dispatch('handleIsOpen');
    };
    return {
      count: computed(() => store.getters.isRoot),
      showMenu,
    };
  },
};
```
## Vuex流程

在組件內執行`store.dispatch('handleIsOpen');`
呼叫`actions`內新增的 `handleIsOpen()`
這個`handleIsOpen()`會 commit 並傳入值到`mutations`的`andleStateIsOpen`
`andleStateIsOpen`修改`state`的值
再由`getters`回傳新值到組件上,
此時用`computed`才能及時運算


## 拆分 Vuex
在`store`資料夾把`state`﹑`actions`﹑`mutatuins`﹑`getters`拆分,
直接建立檔案`state.js`﹑`actions.js`﹑`mutatuins.js`﹑`getters.js`
內容皆為
```js
export default {
    ...
}
```
並在`store/index.js`內
```js
import { createStore } from 'vuex';
import state from './state.js';
import actions from './actions.js';
import mutatuins from './mutatuins.js';
import getters from './getters.js';
export default createStore({
    state, actions, mutatuins, getters
});
```

## modules
可以將vuex模組化,
我們可以在`store`資料夾內在新增一個資料夾,裡面再新增一個`index.js`.
因為在大型專案裡不可能只創立一筆資料,
所以會用`namespaced: true`, 在編譯時自動去抓取名稱, 避免後續區別麻煩

-store
--+index.js (預設)
--+Auth (新增,名字自取)
----+index.js (新增)

範例:
有一個名稱`token`的字串資料, 使用`hendleStateToken()`並帶入參數值到`setToken()`內更新字串資料,
並使用`getToken`取得資料.

```js
export default {
  namespaced: true,  // 一定要開啟
  state: {
    token: '',
  },
  actions: {
    hendleStateToken(context, newToken) {
      context.commit('setToken', newToken);
    },
  },
  mutations: {
    setToken(state, newValue) {
      state.token = newValue;
    },
  },
  getters: {
    getToken(state) {
      return state.token;
    },
  },
};
```

在`store/index.js`下的設定方式

```js
import Auth from './Auth';
export default createStore({
   ...
   modules: {
     Auth,
   },
});
```


此時在組件內應該這樣使用:
一樣使用store.dispatch(),但是裡面的方法名稱必須帶入剛才在`store/index.js`內的`modules`名稱`Auth/hendleStateToken`
如果是用`getters`必須用陣列取值:`store.getters['Auth/getToken']`

```js
  import { useStore } from 'vuex';
  import { onMounted } from 'vue';

  setup() {
    const store = useStore();
    onMounted(() => {
      store.dispatch('Auth/hendleStateToken', '0123456AAA');
      console.log(store.getters['Auth/getToken']);
    });
  },
```
