# 浅谈 Javascript 执行顺序

Javascript 是执行顺序是至上而下的，除非你特别说明，Javascript 代码不会等到页面加载完毕后才执行。比如一个网页里含有以下 HTML 代码：

```
<div id="ele">welcome to www.jb51.net</div>  
```

如果你在这行 HTML 代码前，加入如下 Javascript 代码：

```
<script type="text/javascript">  
  document.getElementById('ele').innerHTML= 'welcome to my blog';  
</script>  
```

运行该页面，你会得到这样的错误信息：“document.getElementById(‘ele') is null。” 原因是，当上面的 javascript 运行时，页面上还没有 ID 为‘ele'的 DOM 元素。

解决办法有两种：

1.把 javascript 代码放在 HTML 代码之后：

```
<div id="ele">welcome to www.jb51.net</div>  
<script type="text/javascript">  
  document.getElementById('ele').innerHTML='welcome to my blog';  
</script>  
```

2.等到在网页加载完毕后，运行该 javascript 代码。你可以使用传统的解决办法（load）：首先加 HTML 的 body 加入 “<body load="load()”>,” 然后在 load()函数里调用上述 javascript 代码。这里要着重介绍的是用 jQuery 来实现：

```
<script>  
$(document).ready(function(){  
   document.getElementById('ele').innerHTML= 'welcome to my blog';  
});  
</script>  
// 当然, 既然用到了 jQuery, 更简单的写法是：  
<script>  
$(document).ready(function(){  
   $('#ele').html('welcome to my blog'); // 这里也可用. text() 方法  
});  
</script>  
```

你可以把上述 jQuery 代码放在页面的任何位置，它总是等到页面加载完毕后才执行。