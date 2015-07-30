# 浅谈 javascript 中的闭包  
  
很长一段时间不理解闭包，后来了解了作用域，以及 this 相关问题才理解了闭包相关知识。

闭包（closure），也是面试题常客。简单点来说就是函数嵌套函数。
函数作为返回值:  
  
```
[js] view plaincopy
function foo () {  
  var a = 1;  
  return function () {  
   a++;  
   console.log(a);  
  }  
}  
var aaa = foo();  
aaa(); //2  
aaa(); //3  
```

其实这个代码不难理解，aaa 是指向 foo() 返回的一个新函数，但是在这个函数里面引用了 a变量，所以当执行完 foo 函数时，变量 a 还存在内存中不释放。即 a 分别为2和3。

函数作为参数：  
  
```
[js] view plaincopy
var a = 10;  
function foo () {  
console.log(a);  
}  
function aaa(fn) {  
 var a = 100;  
 fn();  
}  
aaa(foo);  
```

按照我以前的理解，当执行在 aaa 函数里面执行 fn 函数，那么如果自身没有 a 变量，就去父级作用域找 a 变量，此处是100，那结果是100吗？

可惜答案不是，在这里结果是10，王福朋老师的博客讲的比较好，他说要去创建这个函数的作用域取值，而不是“父作用域”。

## 闭包的使用场景  

因为本人还比较菜鸟，在这里取一个简单例子。当点击 li 的时候弹出 li 在 ul 中所处的位置即索引值。

**html代码：**
[xhtml] view plaincopy
<ul>  
  <li>001</li>  
  <li>002</li>  
  <li>003</li>  
</ul>  


**js代码：**

示例 1：

请看下面的代码，运行后发现，无论点击那个li，结果都是3了。
  
```
[js] view plaincopy
var aLi = document.getElementsByTagName('li');  
for (var i = 0; i<aLi.length; i++) {  
  aLi[i].onclick = function() {  
   alert(i);  
  }  
}  
```

因为在匿名函数里面并没有 i 变量，所以当 for 结束后，我们再去点击页面的 li 标签，此时i 早就是3了。

示例 2：  
  
```
[js] view plaincopy
aLi[i].onclick = (function(i){  
    return function(){  
      alert(i);  
    }  
  })(i);  
```

这次的做法是把函数当返回值，通过自执行函数的参数，把变量 i 传进去，然后因为返回函数要引用这个 i 变量，所以当 for 循环结束也不会释放 i 变量。即在内存中保存了 i 变量的值。基于这样的原理，很容易在低版本 ie 中造成内存泄露。

示例 3：  
  
```
[js] view plaincopy
for (var i = 0; i<aLi.length; i++) {  
  (function(i){  
    aLi[i].onclick = function(){  
      alert(i);  
    }  
  })(i);  
}  
```

这个原理和上面大同小异。

小米前端闭包面试题：  
  
```
[js] view plaincopy
function repeat (func, times, wait) {  
} //这个函数能返回一个新函数，比如这样用  
  
var repeatedFun = repeat(alert, 10, 5000)  
//调用这个 repeatedFun ("hellworld")  
  
//会alert十次 helloworld, 每次间隔5秒  
```

我的答案：  
  
```
[js] view plaincopy
function repeat (func, times, wait) {  
  return function(str) {  
    while (times >0) {  
      setTimeout(function(){  
        func(str);  
      },wait);  
      times--;  
    }  
  }  
}  
  
var repeatedFun = repeat(alert, 10, 100);  
repeatedFun ("hellworld");  
```

以上所述就是本文的全部内容了，希望对大家学习 javascript 闭包能够有所帮助。