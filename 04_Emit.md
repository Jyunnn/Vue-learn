# Vue3.0 emit
---
&nbsp;

如果今天是`Component`要傳遞資料回去父層
就必須用到`emit`

之前有提到`setup()`的參數有`props`
如果要用`emit`就必須再新增一個參數`context`

```js
setup(props, context){
    console.log(context) // 回傳一個物件,裡面有 emit
}
```
&nbsp;
## 使用方式

利用`context.emit(FnName, Data)`
`FnName`為一個回傳資料的名稱
`Data`為想要回傳的資料
下面例子就是名為`main-back`的回傳`num`到父層

```js
// 子組件 Main.vue
setup(props, context) {
    const num = ref(33);
    onMounted(() => {
      context.emit('main-back', num);
    });
    return {
      num,
    };
 },
```
如果不想用 `context`
也可以直接解構
```js
setup(props, { emit }) {
    const num = ref(33);
    onMounted(() => {
      emit('main-back', num.value);
    });
    return {
      num,
    };
 },
```


&nbsp;
父層就可以拿來接收
必須使用一個函數來接收,
函數的第一個參數會是接收過來的資料,
這邊要注意因為我們剛剛在子組件是用`ref`的資料,
如果要取得裡面的值就一定要用`value`這個 key,
或是剛剛在子組件就加上`value`

然後在html的部分,組件會用`@` (`v-on`)來使用回傳資料的名稱事件
剛剛是用`main-back`這個名字,然後使用剛剛新增的函式來接收這個事件
接下來就會在console內看到 33這個數字了


```js
setup() {
    const handlefun = (num) => {
      console.log(num.value);
    };
    return {
      handlefun,
    };
}
```
```html
  <--父層template-->
  <Main @main-back="handlefun" />
```

## emits

子組件也可以對`emit`做驗證,
建立一個`emits`物件,
使用回傳資料的名稱作為`key`
並用一個函式做為`value`,`return`一個boolean
如果回傳的值不為 33,
console就會跳出
> Invalid event arguments: event validation failed for event "main-back".

但是還是會傳遞值給父層,
可以加入`alarm`來使用

```js
emits: {
  'main-back': (num) => num === 33,
},
setup(props, { emit }) {
  const num = ref(33);
  onMounted(() => {
    emit('main-back', num.value);
  });
  return {
    num,
  };
},
```
