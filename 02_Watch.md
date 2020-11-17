# Vue3.0 開始
---
&nbsp;
`watch`可以監聽值的變化
一樣要從`Vue`裡面取用
```js
const { ref, reactive, watch } = Vue; 
```

開始時有提到`ref`和`reactive`其中一個差別是監聽的深度

```js
let time = reactive({ data: 0 })
setInterval(()=>{ time.data++ }, 1000) //每一秒就加 1
watch(time ,(newval, oldval)=>{ //參數:新值,舊值
    console.log(newval, oldval)
})
```
這時候會發現Console內顯示的新舊值都一樣
```js
Proxy {data: 1} > Proxy {data: 1}
Proxy {data: 2} > Proxy {data: 2}
```
因為我們今天是要監控裡面單一的`key`,所以必須改為`time.data`
```js
watch(time.data ,(newval, oldval)=>{
    console.log(newval, oldval)
})
```
但這時候又會報錯

> Invalid watch source:  0 A watch source can only be a getter/effect function, a ref, a reactive object, or an array of these types. 

[官方文件](https://v3.vuejs.org/guide/reactivity-computed-watchers.html#watch)其實有提到,必須是一個回傳值
```js
watch(()=> time.data ,(newval, oldval)=>{
    console.log(newval, oldval)
})
```
這樣才會正常回傳新值和舊值

### deep

`ref`使用`watch`就有些不同,用`reactive`的方式作用的話,只能監聽到`key`裡面的一個`value`
此時就可以利用`watch`的第三個參數`{deep: true}`,監聽整個`ref`
```js
let time = ref({data: 0})

setInterval(()=>{
    time.value.data++
},1000)

watch(()=> time.value ,(newval, oldval)=>{
    console.log(newval, oldval)
},{deep: true})
```