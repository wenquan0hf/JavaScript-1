# 浅谈 javascript 的分号的使用  
  
JS 中 function 的开头有必要加分号吗？js 语句后应该加分号吗？ javascript 大括号后面应使用分号吗？JS 中 function 的开头有加感叹号、分号是什么意思呢？
Js 多个文件集成成一个文件后，压缩代码时避免发生语法错误，可以如下处理

## js 前加分号

例如：;(function($){...此处代码...})();

Javascript 中分号表示语句结束，在开头加上，是为了压缩的时候和别的方法分割一下，表示一个新的语句开始  

## js函数后加分号

例如  
  
```
[js] view plaincopy
// 模块1  
// 前面有若干代码  
var Manager = {  
 prop: '',  
 method: function () {  
  
 }  
}  
// 模块2，开头是个立即执行函数  
(function () {  
 // 代码  
})()  
```

经过压缩后变成： }}(function 那里，会被当成一个函数来执行，于是整体的解析就会出错了  
  
```
[js] view plaincopy
var Manager = {prop: '',method: function (){}}(function () {})()  
```  

解决方法： 是在 Manager 函数后加分号

以上所述就是本文的全部内容了，希望大家能够喜欢。