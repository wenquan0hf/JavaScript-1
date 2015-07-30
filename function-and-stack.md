# 浅谈 JavaScript 函数与栈

Javascript 中会经常用到 setTimeout 来推迟一个函数的执行，如：

```
setTimeout(function(){  
    alert("Hello World");  
},1000);  
```

会在执行到这句话后延迟 1 秒钟来弹出 alert 窗口。

那么再看这一段：

```
function test(){  
    setTimeout(function() {alert(1)}, 0);  
    alert(2);  
}  
test();  
```

注意这段代码中的 setTimeout 延迟设为了 0，就是延迟 0 毫秒，貌似是不做任何延迟立刻执行，即 1，2。但实际的执行结果确是 2，1。为什么？这得从 Javascript 调用堆栈(call stack)和 setTimeout 的功能说起。

首先，JavaScript 是单线程的，即同一时间只执行一条代码，所以每一个 JavaScript 代码执行块会 “阻塞” 其它异步事件的执行。其次，和其他的编程语言一样，Javascript 中的函数调用也是通过堆栈实现的。在执行函数 test 的时候，test 先入栈，如果不给 alert(1)加 setTimeout，那么 alert(1)第 2 个入栈，最后是 alert(2)。但现在给 alert(1)加上 setTimeout 后，alert(1)就被加入到了一个新的堆栈中等待，并 “尽可能快” 的执行。这个尽可能快就是指在 a 的堆栈完成后就立刻执行，因此实际的执行结果就是先 alert(2)，再 alert(1)。在这里 setTimeout 实际上是让 alert(1)脱离了当前函数调用堆栈。

看下面一个例子：

```
<input name="input" onkeydown="alert(this.value)" type="text" value="a" />  
```

这样一段函数意图是每输入一个字符就把当前 input 里的所有字符都 alert 出来，但实际效果确是 alert 出按键之前的内容。这里，我们就可以利用 setTimeout(0) 来实现。

```
<input onkeydown="var me=this; setTimeout(function(){alert(me.value)}, 0)" name="input" type="text" value="a" />  
```

这样当 onkeydown 事件触发的时候，alert 就被放入了下一个调用堆栈，一旦 onkeydown 事件触发的堆栈关闭后就开始执行。当然浏览器还有个 onkeyup 事件也可以实现我们的需求。


这样的 setTimeout 用法在实际项目中还是会时常遇到。比如浏览器会聪明的等到一个函数堆栈结束后才改变 DOM，如果再这个函数堆栈中把页面背景先从白色设为红色，再设回白色，那么浏览器会认为 DOM 没有发生任何改变而忽略这两句话，因此我们可以通过 setTimeout 把 “设回白色” 函数加入下一个堆栈，那么就可以确保背景颜色发生过改变了（虽然速度很快可能无法被察觉）。


总之，setTimeout 增加了 Javascript 函数调用的灵活性，为函数执行顺序的调度提供极大便利。