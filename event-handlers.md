# 浅谈 Javascript 事件处理程序的几种方式

事件就是用户或浏览器自身执行的某种动作。比如说 click，mouseover，都是事件的名字。而相应某个事件的函数就叫事件处理程序（或事件侦听器）。为事件指定处理程序的方式有好几种。 

## HTML 事件处理程序。
 
如: 

```
<script type="text/javascript">   
function show(){   
alert('hello world!');   
}   
</script>   
<input type="button" value="click me" onclick="show()"/>  
```

相信这种方式是目前咱们大家用得比较多的一种，但是在 html 中指定事件处理程序有两个缺点。 
1. 首先：存在一个时差问题。就本例子来说，假设 show()函数是在按钮下方，页面的最底部定义的，如果用户在页面解析 show()函数之前就单击了按钮，就会引发错误； 
2. 第二个缺点是 html 与 javascript 代码紧密耦合。如果要更换时间处理程序，就要改动两个地方：html 代码和 javascript 代码。 

因此，许多开发人员摒弃 html 事件处理程序，转而使用 javascript 指定事件处理程序。 

## Javascript 指定事件处理程序 

javascript 指定事件处理程序包括三种方式： 


1.DOM0 级事件处理程序 

如： 

```
var btn=document.getElementById("mybtn"); // 取得该按钮的引用   
btn.onclick=function(){   
alert('clicked');   
alert(this.id); // mybtn  
```

以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理。 

删除 DOM0 级方法指定的事件处理程序： 

```
btn.onclick=null; // 删除事件处理程序 
} 
```


2.DOM2 级事件处理程序 

DOM2 级事件定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener() 和 removeEventListener()。所有 DOM 节点中都包含这两个方法，并且它们都接受 3 个参数：要处理的事件名，做为事件处理程序的函数和一个布尔值。最后这个参数如果是 true，表示在捕获阶段调用事件处理程序；如果是 fasle，表示在冒泡阶段调用事件处理程序。 

如： 

```
var btn=document.getElementById("mybtn");   
btn.addEventListener("click",function(){   
alert(this.id);   
},false);  
```

使用 DOM2 级事件处理程序的主要好处是可以添加多个事件处理程序。 

如： 

```
var btn=document.getElementById("mybtn");   
btn.addEventListener("click",function(){   
alert(this.id);   
},false);btn.addEventListener("click",function(){ <br>alert("hello world!"); <br>},false); <br>  
```

这里为按钮添加了两个事件处理程序。这两个事件处理程序会按照它们的顺序触发。 

通过 addEventListener()添加的时间处理程序只能使用 removeEventListener()来移除，移除时传入的参数与添加时使用的参数相同。这也意味着通过 addEventListener()添加的匿名函数将无法移除。 

如： 

```
var btn=document.getElementById("mybtn");   
btn.addEventListener("click",function(){   
alert(this.id);   
},false);// 移除 <br>btn.removeEventListener("click",function(){// 这样写没有用 （因为胃第二次与第一次相比是一个完全不同的函数） <br>alert(this.id); <br>},false); <br>  
```

解决办法： 

```
var btn=document.getElementById("mybtn");   
var hander=function(){   
alert(this.id);   
};   
btn.addEventListener("click",hander,false);   
  
btn.removeEventListener("click",hander,false); // 有效  
```

注意：这里我们的第三个参数都是 false，是在冒泡阶段添加的。大多数情况下，都是就事件处理程序添加到事件流的冒泡阶段，这样可以最大限度的兼容各种浏览器。 

## IE 事件处理程序 

IE 实现了与 DOM 中类似的两个方法: attachEvent()和 detachEvent()。这两个方法接受相同的两个参数：事件处理程序名称和事件处理程序函数。由于 IE 只支持时间冒泡，所有通过 attachEvent() 添加的事件处理程序都会被添加包冒泡阶段。 

如： 
 
```
var btn=document.getElementById("mybtn");   
btn.attachEvent("onclick",function(){   
alert("clicked");   
})  
```

注意：attachEvent()函数的第一个参数是"onclick"，而非 DOM 的 addEventListener()中的 "click"。 
attachEvent()方法也可以用来为一个元素添加多个事件处理程序。
 
如： 

```
var btn=document.getElementById("mybtn");   
btn.attachEvent("onclick",function(){   
alert("clicked");   
});   
btn.attachEvent("onclick",function(){   
alert("hello world!");   
});  
```

这里调用了两次 attachEvent()，为同一个按钮添加了两个不同的事件处理程序。不过，与 DOM 方法不同的是，这些事件处理程序不是以它们的添加顺序执行，而是以相反的顺序被触发。单击这个例子中的按钮：首先看到的是"hello world", 然后才是"clicked"。
使用 attachEvent()添加的事件可以通过 detachEvent()来移除，条件是必须提供相同的参数。 

```
var btn=document.getElementById("mybtn");   
var hander=function(){   
alert("clicked");   
}   
btn.detachEvent("onclick",hander}); // 移除  
```

以上三种方式为目前的主要的事件处理程序方式，那看到这里你肯定会想到，既然不同的浏览器会有不同的差异，那怎么保证跨浏览器的事件处理程序呢？ 

为了以跨浏览器的方式处理事件，不少的开发人员是使用能够隔离浏览器差异性的 Javascript 库，还有一些开发人员会自己开发最合适的事件处理方法。 

这里提供一个 EventUtil 对象, 可以用这个对象来处理浏览期间的差异： 


```
var EventUtil = {   
addHandler: function(element, type, handler){ // 该方法接受 3 个参数：要操作的元素，事件名称和事件处理程序函数   
if (element.addEventListener){ // 检查传入的元素是否存在 DOM2 级方法   
element.addEventListener(type, handler, false); // 若存在，则使用该方法   
} else if (element.addEvent){ // 如果存在的是 IE 的方法   
element.attachEvent("on" + type, handler); // 则使用 IE 的方法，注意，这里的事件类型必须加上 "on" 前缀。   
} else { // 最后一种可能是使用 DOM0 级   
element["on" + type] = hander;   
}   
},   
  
removeHandler: function(element, type, handler){ // 该方法是删除之前添加的事件处理程序   
if (element.removeEventListener){ // 检查传入的元素是否存在 DOM2 级方法   
element.removeEventListener(type, handler, false); // 若存在，则使用该方法   
} else if (element.detachEvent){ // 如果存在的是 IE 的方法   
element.detachEvent("on" + type, handler); // 则使用 IE 的方法，注意，这里的事件类型必须加上 "on" 前缀。   
} else { // 最后一种可能是使用 DOM0 及方法 (在现代浏览器中，应该不会执行这里的代码)   
element["on" + type] = null;   
}   
}   
};  
```

可以像下面这样使用 EventUtil 对象: 

```
var btn =document.getElementById("mybtn");   
var hander= function(){   
alert("clicked");   
};   
// 这里省略了部分代码   
EventUtil.addHandler(btn,"click",hander);   
// 这里省略了部分代码   
EventUtil.removeHandler(btn,"click",hander); // 移除之前添加的事件处理程序  
```

可见，使用 addHandler 和 removeHandler 来添加和移除事件处理程序还是很方便的。