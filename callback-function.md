# 浅谈 javascript 回调函数

把函数作为参数传入到另一个函数中。这个函数就是所谓的**回调函数**。

经常遇到这样一种情况，某个项目的 A 层和 B 层是由不同的人员协同完成。A 层负责功能 funA，B 层负责 funcB。当 B 层要用到某个模块的数据，于是他对 A 层人员说，我需要你们提供满足某种需求的数据, 你给我提供一个接口。

A 层的人员说: 我给你提供数据，怎么展示和处理则是 B 的事情。

当然 B 层不可能为你每个需求都提供一个数据接口，B 给 A 提供一个通过的接口。B 得到数据，然后 B 写函数去展示。

即，你需要和其他人合作，别人提供数据，而你不需要关注别人获取或者构建数据的方式方法。你只要对这个拿到的数据进行操作。这时候就需要使用回调函数

因此，回调本质上是一种设计模式，并且 jQuery(包括其他框架)的设计原则遵循了这个模式。

一个同步(阻塞)中使用回调的例子，目的是在 func1 代码执行完成后执行 func2。

```
var func1=function(callback){  
  
    //do something.  
  
    (callback && typeof(callback) === "function") && callback();  
  
}  
func1(func2);  
  
    var func2=function(){  
  
}  
```

异步回调的例子：

```
$(document).ready(callback);  
    $.ajax({  
  
        url: "test.html",  
  
        context: document.body  
  
    }).done(function() {   
  
        $(this).addClass("done");  
  
    }).fail(function() {   
  
        alert("error");  
  
    }).always(function() {   
  
        alert("complete");   
  
    });  
```

注意的是，ajax 请求确实是异步的, 不过这请求是由浏览器新开一个线程请求, 当请求的状态变更时, 如果先前已设置回调, 这异步线程就产生状态变更事件放到 JavaScript 引擎的处理队列中等待处理。见：[http://www.phpv.net/html/1700.html](http://www.phpv.net/html/1700.html)

## 回调什么时候执行

回调函数，一般在同步情境下是最后执行的，而在异步情境下有可能不执行，因为事件没有被触发或者条件不满足。

## 回调函数的使用场合

资源加载：动态加载 js 文件后执行回调，加载 iframe 后执行回调，ajax 操作回调，图片加载完成执行回调，AJAX 等等。

DOM 事件及 Node.js 事件基于回调机制(Node.js 回调可能会出现多层回调嵌套的问题)。

setTimeout 的延迟时间为 0，这个 hack 经常被用到，settimeout 调用的函数其实就是一个 callback 的体现

链式调用：链式调用的时候，在赋值器(setter) 法中(或者本身没有返回值的方法中)很容易实现链式调用，而取值器(getter)相对来说不好实现链式调用，因为你需要取值器返回你需要的数据而不是 this 指针，如果要实现链式方法，可以用回调函数来实现。

setTimeout、setInterval 的函数调用得到其返回值。由于两个函数都是异步的，即：他们的调用时序和程序的主流程是相对独立的，所以没有办法在主体里面等待它们的返回值，它们被打开的时候程序也不会停下来等待，否则也就失去了 setTimeout 及 setInterval 的意义了，所以用 return 已经没有意义，只能使用 callback。callback 的意义在于将 timer 执行的结果通知给代理函数进行及时处理。

网上收集一下资料，应该弄懂了，自己整理出一个例子：

```
function fun(num,callback){  
  
    if(num<0)  {   
  
        alert("调用 A 层函数处理!");  
  
        alert("数据不能为负, 输入错误!");   
  
    }else if(num==0){  
  
        alert("调用 A 层函数处理!");  
  
        alert("该数据项不存在！");  
  
    }else{  
  
        alert("调用 B 层函数处理!");  
  
        callback(1);  
  
    }  
  
}  
  
function test(){  
  
    var num=document.getElementById("score").value;  
  
    fun(num,function(back){ // 匿名 B 层处理函数  
  
　　　　alert(":"+back);  
  
        if(num<2)   
  
            alert("数字为 1");  
  
        else if(num<=3)   
  
            alert("数字为 2 或 3！");  
  
        else   
  
            alert("数字大于 3!");   
  
    })  
  
 }  
```

当函数开始执行 fun 的时候，先跑去找判定 num 是否是负数或者为零，否则执行 B 层处理函数 alert(":"+back)；输出 1，判定为 <2、<=3、>3 等情况。

## 经验小提示：

最好保证回调存在且必须是函数引用或者函数表达式：

```
(callback && typeof(callback) === "function") && callback();
[js] view plaincopy
var obj={  
  
        init : function(callback){  
  
        //TODO ...  
  
        if(callback && typeof(callback) === "function") && callback()){  
  
              callback('init...');// 回调  
  
        }  
  
    }  
```

最后，关于为什么要使用回调函数呢？下面的比喻很生动有趣。

你有事去隔壁寝室找同学，发现人不在，你怎么办呢？

方法 1，每隔几分钟再去趟隔壁寝室，看人在不

方法 2，拜托与他同寝室的人，看到他回来时叫一下你

前者是轮询，后者是回调。

那你说，我直接在隔壁寝室等到同学回来可以吗？

可以啊，只不过这样原本你可以省下时间做其他事，现在必须浪费在等待上了。把原来的非阻塞的异步调用变成了阻塞的同步调用。

JavaScript 的回调是在异步调用场景下使用的，使用回调性能好于轮询。

更简单一点：

“我现在出发，到了通知你”

这是一个异步的流程，“我出发” 这个过程中（函数执行），“你” 可以去做任何事，“到了”（函数执行完毕）“通知你”（回调）进行之后的流程。