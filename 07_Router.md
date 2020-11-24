# Vue3.0 router
---
&nbsp;
Router設定會在`src/router/index.js`裡面,
剛開始都會幫你預載好.
&nbsp;
## Router History modes
在使用`npm run build`後, 使用LiveServer首頁的部分可能會有問題, 因為`router`設定首頁是用`/`, LiveServer會把首頁導向index.html
此時可以暫時嘗試切換為`createWebHistory`, 不過不建議切換, 可以修改後端設定.
[參考文件](https://next.router.vuejs.org/guide/essentials/history-mode.html#hash-mode)
&nbsp;
## 404 Page
可以再最後面加入一個`NotFoundComponent`的router
```js
import NotFoundComponent from '../views/404.vue';
const routes = [
    ...,
    {
      path: '/:pathMatch(.*)',
      component: NotFoundComponent,
    },
]
```
&nbsp;
## Component

`component`有兩種寫法,差別在於網站先載入資料和進入連結網站時才載入,
如果是用`import`方式,就是預先載入,但如果這個頁面有很多資料要讀取,進入網頁的速度就很慢
```js
import Home from '../views/Home.vue';
{
    path: '/',
    name: 'Home',
    component: Home,
},
```
如果是用`component() => {...}`的方式,就是`lazy-loaded`
```js
import Home from '../views/Home.vue';
{
    path: '/product',
    name: 'product',
    component: () => import('../views/Products/Index.vue'),
},
```
&nbsp;
## 動態路由

views資料夾內含有子資料夾Products,底下有兩個vue文件
|＋views
|－－＋Products
|－－－－_id.vue
|－－－－index.vue

_id為我要作為動態Router的頁面,所以路由必須加上`:`,
`name`自己取名,`component`回傳_id.vue
```js
  {
    path: '/product/:id',
    name: 'product_id',
    component: () => import('../views/Products/_id.vue'),
  }
```
在`<template>`的部分,因為是動態路由,所以一樣要加上`:`,
我會在取得商品資料的`json`內,每一個商品都有一個獨一無二的`id`的屬性,
這樣才能讓路由順利取得商品資料
```html
<router-link :to="`/product/${product.id}`">
```
