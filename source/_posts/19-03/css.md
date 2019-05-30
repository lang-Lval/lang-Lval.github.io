---
title: css简单初始化
tags: css
categories: css
date: 2019-01-29 13:45:03
---

### css配合rem、
现在基本都不会用rem了，但偶尔rem配合vw、vh，啥的也能有一些用武之地

```
function responsive() {
    //屏幕的宽度
    var screenWidth = window.innerWidth || document.documentElement.clientWidth;

    if(screenWidth <= 320) {
    screenWidth = 320;
    }

    if(screenWidth >= 750) {
    screenWidth = 750;
    }

    //希望把设计图分成了15rem   750的设计
    //如果设计图是640的设计图   分成16份  1rem = 40px
    var count = 15;

    //设置html的字体大小 如750的设计图此时html字体大小为50，也就是1rem=50px;在less中，可定义一个变量@r:50rem，下面定义样式是就可以用：设计图量出的大小/@r
    document.documentElement.style.fontSize = (screenWidth/count).toFixed(2) + "px";
}

responsive();
```

### vue初始化css
<!--more-->
```
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  width: 100%;
  height: 100%;
  font-size: 15px;
}
*{
  margin: 0;
  padding: 0;
}
html,body {
  height: 100%;
}
input {
  background:none;  
	outline:none;  
	border:0px;
}
input:focus, textarea:focus {

  outline: none;

}
ul {
  list-style: none;
}
input::-webkit-input-placeholder{
  /* input内的字体要比placeholder设置的字体大，不然会被切割 */
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
} 
input:-moz-placeholder{
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
}  
input::-moz-placeholder{
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
} 
input:-ms-input-placeholder{
  font-size: 14px;
  line-height: 20px;
  font-weight: 400;
} 
```