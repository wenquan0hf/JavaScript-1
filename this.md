# 浅谈 javascript 中 this 在事件中的应用  
  
this 关键字在 javascript 中是非常强大的,但是如果你不清楚它是怎么工作的就很难使用它.
  
```
[js] view plaincopy
function dosomething(){ this.style.color="#fff"; }  
```  

上面这段代码中的 this 指向什么呢,运行 dosomething() 会输出什么呢?

在 javascript 中,this 总是指向当前执行的这个函数,或者把函数作为方法调用的这个对象.当我们在页面上定义 dosomething() 这个方法后,this 的所有者就是当前的页面,或者说是全局对象.

所以我们执行 dosomething() 这个函数,会引发错误.因为函数的 this 指向的是全局对象window,而 window 对象没有 style 属性.

复制:
  
```
[js] view plaincopy
element.onclick=dosomething;  
```  

dosomething() 现在被整个复制到 onclick 属性上作为一个方法.所以如果这个事件执行的话,this 就指向这个 HTML 元素,相应 HTML 元素的 color 就会改变.dosomething 每次复制到事件上,this 就会指向当前执行这个方法的 html 元素.

引用:
  
```
[js] view plaincopy
<element onclick="dosomething()">  
```  

此时你没有复制这个方法,而是引用了这个方法,onclick 属性并不包含实际的方法,仅仅只是一个方法的调用.当我们执行这个方法时,this 再次指向全局 window 对象并引发错误.

以上就是本文的全部内容了，有需要的小伙伴好好来研究下吧。