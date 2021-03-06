---
title: react适配H5的scss
date: 2019-10-05
sidebar: auto
showSponsor: true
categories:
  - react
tags:
  - react
---
## 将下列的scss引入到自己的scss 并且单位不再是 px换成 Rm()

```scss
@charset "UTF-8";
$yh:"Microsoft yahei";
// 这里的24 针对750的h5
// 14.0625 是针对 350的页面
@function Rm($px, $base: 24) {
    @return ($px / $base) * 1rem;
}

// @function pxToRemr($px, $base: 14.0625) {
//   @return ($px / $base) * 1rem;
// }
html {
    font-size: 62.5%;
    font-family: $yh;
}

body,
textarea,
input,
select,
option {
    color: #333;
    font-family: "Hiragino Sans GB", "Microsoft Yahei", tahoma, arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -webkit-tap-highlight-color: transparent;
}

body,
h1,
h2,
h3,
h4,
h5,
h6,
blockquote,
ol,
ul,
dl,
dd,
p,
textarea,
input,
select,
option,
form {
    margin: 0;
    padding: 0;
}

ol,
ul,
textarea,
input,
option,
th,
td {
    padding: 0;
}

.page {
    min-width: 320px;
    max-width: 750px;
    margin: 0 auto;
}

h1,
h2,
h3,
h4,
h5,
h6 {
    font-weight: normal;
    font-size: 100%;
}

a,
select,
input,
textarea {
    outline: none;
}

article,
aside,
details,
figcaption,
figure,
footer,
header,
menu,
nav,
section {
    display: block;
}

table {
    border-collapse: collapse;
    border-spacing: 0;
}

ul,
ol {
    list-style-type: none;
}

.hide {
    display: none;
}

.show {
    display: block;
}

.clearfix:after {
    content: '.';
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}

//.clearfix {*zoom:1;}
.clear {
    clear: both;
    height: 0;
    overflow: hidden;
}

/* ios默认文本框阴影 */
input[type=text],
textarea {
    -webkit-appearance: none;
}

/* 低版本安卓文本框层级问题 */
input:focus {
    -webkit-user-modify: read-write-plaintext-only;
}

fieldset,
img {
    border: 0;
}

a {
    text-decoration: none;
}

a,
textarea,
input {
    outline: none;
}

blockquote,
q {
    quotes: none;
}

blockquote:before,
blockquote:after,
q:before,
q:after {
    content: '';
    content: none;
}

textarea {
    overflow: auto;
    resize: none;
}

// 以下是按照320下12像素字号走的，因为谷歌不识别小于12号字号
@media only screen and (min-width: 320px) {
    html {
        font-size: 75% !important;
        /* 12÷16=75% */
    }
}

/**
    62.5%   10px;
    640 150%    24px;
    320除以标准比例 640  再乘以 640的基数24  再除以  HTML 根据基数16
    320/640  * 24 / 16 = 75%;
    375/640  * 24 / 16 = 87.89%;
    414/640  * 24 / 16 = 97.03%
*/
@media only screen and (min-width: 360px) {
    html {
        font-size: 84.3% !important;
        /* 13.5÷16=84.3% */
    }
}

@media only screen and (min-width: 375px) {
    html {
        font-size: 87.890625% !important;
        /* 14.0625÷16=87.890625% */
    }
}

@media only screen and (min-width: 384px) {
    html {
        font-size: 90% !important;
        /* 14.4÷16=90% */
    }
}

@media only screen and (min-width: 390px) {
    html {
        font-size: 91.4% !important;
        /* 14.625÷16=91.4% */
    }
}

@media only screen and (min-width: 412px) {
    html {
        font-size: 96.56% !important;
        /* 15.45÷16=96.56% */
    }
}

@media only screen and (min-width: 414px) {
    html {
        font-size: 97.03% !important;
        /* 15.525÷16=97.03% */
    }
}

@media only screen and (min-width: 480px) {
    html {
        font-size: 112.5% !important;
        /* 18÷16=112.5% */
    }
}

@media only screen and (min-width: 560px) {
    html {
        font-size: 131.25% !important;
        /* 21÷16=131.25% */
    }
}

@media only screen and (min-width: 640px) {
    html {
        font-size: 150% !important;
        /* 24÷16=150% */
    }
}

@media only screen and (min-width: 720px) {
    html {
        font-size: 168.75% !important;
        /* 27÷16=168.75% */
    }
}

@media only screen and (min-width: 750px) {
    html {
        font-size: 175.78125% !important;
        /* 28.125÷16=175.78125% */
    }
}

// @media only screen and (min-width: 800px){
//     html {
//         font-size: 187.5%!important; /* 30÷16=146.43% */
//     }
// }
// @media only screen and (min-width: 960px){
//     html {
//         font-size: 225%!important; /* 36÷16=146.43% */
//     }
// }
```

## react按需加载 js 文件 （适合大项目）
- 两个js文件放在src同级
```js
// Bundle.js
// 一个 按需加载组件
import React from 'react';

export default class Bundle extends React.Component {
   state = {
      mod: null
   };
   componentDidMount() {
      this.load(this.props);
   }
   componentDidUpdate(nextProps) {
      if (nextProps.load !== this.props.load) {
         this.load(nextProps);
      }
   }
   // load 方法，用于更新 mod 状态
   load(props) {
      // 初始化
      this.setState({
         mod: null
      });
      /*
         调用传入的 load 方法，并传入一个回调函数
         这个回调函数接收 在 load 方法内部异步获取到的组件，并将其更新为 mod
      */

      // props.load(mod => {
      //    this.setState({
      //       mod: mod.default ? mod.default : mod
      //    });
      // });
      props.load().then(mod => {
         this.setState({
            mod: mod.default ? mod.default : mod
         });
      });
   }

   render() {
      /*
         将存在状态中的 mod 组件作为参数传递给当前包装组件的'子'
      */

      return this.state.mod ? this.props.children(this.state.mod) : null;
   }
}

```
```js
// lazyLoad.js
// 懒加载方法
import React from 'react';
import Bundle from './Bundle';

// 默认加载组件，可以直接返回 null
const Loading = () => <div>Loading...</div>;

/*
   包装方法，第一次调用后会返回一个组件（函数式组件）
   由于要将其作为路由下的组件，所以需要将 props 传入
*/

const lazyLoad = loadComponent => props => (
   <Bundle load={loadComponent}>
      {Comp => (Comp ? <Comp {...props} /> : <Loading />)}
   </Bundle>
);

export default lazyLoad;
```
- 使用的时候将文件引入 并且导入组件的时候也要改写
```js
import lazyLoad from './lazyLoad'
const Home = lazyLoad(() => import('./view/home/'))
```

## css pc 版默认样式
```css
//\5b8b\4f53
//$yh:'Microsoft Yahei';
body, textarea, input, select, option {
  font-size: 14px;
  color: #333;
  font-family: "Hiragino Sans GB", "Microsoft Yahei", tahoma, arial, sans-serif;
  -webkit-font-smoothing: antialiased;
}
h1, h2, h3, h4, h5, h6 {
  font-size: 100%;
}
body, h1, h2, h3, h4, h5, h6, blockquote, ol, ul, dl, dd, p, textarea, input, select, option, form {
  margin: 0;
}
ol, ul, textarea, input, option, th, td {
  padding: 0;
}
table {
  border-collapse: collapse;
}
li {
  list-style-type: none;
}
article, aside, details, figcaption, figure, footer, header, menu, nav, section {
  display: block;
}
.clearfix:after {content:'.';display:block;height:0;clear:both;visibility:hidden;}
.clearfix {*zoom:1;}
.clear {
  clear: both;
  height: 0;
  overflow: hidden;
}
a {
  text-decoration: none;
  color: #333;
}
a, textarea, input, select {
  outline: none;
}
textarea {
  overflow: auto;
  resize: none;
}
.img img {
  display: block;
}
a img {
  border: none;
}
//.z-index {
//  position: fixed;
//  _position: absolute;
//  z-index: 999;
//  display: none;
//  overflow: hidden;
//}
.pr {
  position: relative;
}
.pa {
  position: absolute;
}
.fl {
  float: left;
}
.fr {
  float: right;
}
a:hover {
  text-decoration: underline;
}
.m {
  margin: 0 auto;
  width: 1200px;
  padding:0 40px;
}
.mt {
  margin-top: 15px;
}
.hide {
  display: none;
}
.show {
  display: block;
}
/**
去ie10 小X
*/
::-ms-clear { display: none;}
::-ms-reveal { display: none;}
/**
 placeholder color更改
*/
::-webkit-input-placeholder { /* WebKit browsers */
    color:#ccc;
}
:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    color:#ccc;
}
::-moz-placeholder { /* Mozilla Firefox 19+ */
    color:#ccc;
}
:-ms-input-placeholder { /* Internet Explorer 10+ */
    color:#ccc;
}
[v-cloak] {
  display: none;
}
```