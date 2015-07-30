# 浅谈 JavaScript 之事件绑定


其实没有什么新的知识点，只是为了方便其他有需要的朋友们翻阅，对自己而言也算是一个积累，所以只能算是闲谈 JavaScript，老鸟们可以尽情飘过。

在进入正题之前，先提个问题热热身吧。

**现在有如下 HTML 结构**：

```
<div id="wrap">  
 <input type="button" value="按钮一" />  
 <input type="button" value="按钮二" />  
 <input type="button" value="按钮三" />  
 <input type="button" value="按钮四" />  
 <input type="button" value="按钮五" />  
</div>  
```

**以及如下 JavaScript 代码**：

```
var wrap = document.getElementById('wrap'),   
    inputs = wrap.getElementsByTagName('input');   
  
for (var i = 0, l = inputs.length; i <l; i++) {   
    inputs[i].onclick = function () {   
        alert(i);   
    }   
}  
```

**请问，这样执行的结果是什么？**

/*************************** 分割线 ***************************/

如果你的回答是 “点击按钮时， alert 当前按钮的索引值 i”，那你就中了我的圈套了。大家不妨试试，无论你点击哪个按钮，它都会 alert(5)。

这个看似理所当然的结果为什么会和实际情况不同呢？其实也是很好理解的。
因为 onclick 只是事件绑定，而不是执行，当我们执行 onclick 事件的时候，这时的 i 已经是循环以后的值了，照这样看，每个按钮都 alert(5) 也就不足为奇了。

那么，如果我们要怎么实现 “点击按钮时，alert 当前按钮的索引值 i” 呢？这里就要用到 JavaScript 中暗藏玄机的一个概念 “闭包”。我们可以用闭包的方式改写以上 JS，把 for 循环中的 i 值保存在内存中，**代码如下**：

```
var wrap = document.getElementById('wrap'),   
    inputs = wrap.getElementsByTagName('input');   
  
for (var i = 0, l = inputs.length; i <l; i++) {   
    (function (cur) {   
        inputs[cur].onclick = function () {   
            alert(cur);   
        }   
    })(i)   
}  
```

再试试效果？确实能 alert 出相应的索引值了，不过至此为止还只是开胃菜，正题才刚刚开始！

以上的方法，我们是通过循环 + 闭包给 button 按钮上绑定事件，我们知道，在 JavaScript 中函数也是对象，对象就会占用内存，现在的例子中只有 5 个按钮，或许你会认为这样的性能开销可以忽略不计，但是如果当我们有 50 个，甚至 500 个按钮的时候，IE 已经哭了，当有更多其他性能隐患并发时，所有的浏览器都哭了。

回到刚才的例子，我们可以用 “事件委托” 的方法来解决这个因绑定事件随着按钮增加而可能导致的性能问题。原理很简单，利用 Javascript 的事件冒泡，我们可以把事件的绑定从按钮移到它们的父级元素上，不管按钮有多少，它们只有一个共同的父级元素，那样我们只需要绑定一次事件就可以了。

**代码如下**：

```
var wrap = document.getElementById('wrap'),   
    inputs = wrap.getElementsByTagName('input');   
  
wrap.onclick = function (ev) {   
    var ev = ev || window.event,   
    target = ev.target || ev.srcElement;   
    for (var i = 0, l = inputs.length; i <l; i++) {   
        if (inputs[i] === target) {   
            alert(i)   
        }   
    }   
}  
```

至此，正餐完毕，我们还可以再深入一下，来些餐后甜点。

除了在性能上，事件委托比闭包的事件绑定更有优势以外，事件委托还无需顾及子元素（即被绑定事件的元素）的数量。比如，我们在 onclick 事件绑定以后，**增加一个按钮**：

```
var newInput = document.createElement('input');   
newInput.setAttribute('type', 'button');   
newInput.setAttribute('value', '按钮六');   
wrap.appendChild(newInput);  
```

同样在最后加了这段代码的闭包方式和事件委托方式，我们可以看到，闭包实现的事件绑定中点击 “按钮六” 毫无效果，但是在事件委托中实现的事件绑定点击 “按钮六” 则会有 alert。相反，如果我们要删除一个按钮，闭包的方式仍会在内存中保存已删除按钮的 onclick 事件（除非手动设为 null），事件委托则不会对内存造成多余的负担，就为这个原因，我们也应该多加利用事件委托的方式来绑定同一层级的多个元素。
