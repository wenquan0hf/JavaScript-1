# 浅谈 javascript 函数劫持 [转自 xfocus] 第 1/3 页

## 概述 

javascript 函数劫持，也就是老外提到的 javascript hijacking 技术。最早还是和剑心同学讨论问题时偶然看到的一段代码，大概这样写的： 

```
window.alert = function(s) {}; 
```

觉得这种用法很巧妙新颖，和 API Hook 异曲同工，索性称之为 javascript function hook，也就是函数劫持。通过替换 js 函数的实现来达到劫持这个函数调用的目的，一个完整的 hook alert 函数例子如下： 

```
<!--1.htm--> 
[js] view plaincopy
<script type="text/javascript">   
<!--   
var _alert = alert;   
window.alert = function(s) {   
if (confirm("是否要弹框框，内容是 \"" + s + "\"？")) {   
_alert(s);   
}   
}   
//-->   
</script>   
<html>   
<body>   
<input type="button" onclick="javascript: alert('Hello World!')" value="test" />   
</body>   
</html>  
```

搞过 API Hook 的同学们看到这个代码一定会心的一笑，先保存原函数实现，然后替换为我们自己的函数实现，添加我们自己的处理逻辑后最终再调用原来的函数实现，这样这个 alert 函数就被我们劫持了。原理非常简单，下面举些典型的应用来看看我们能利用它来做些什么。 

## 应用举例 

1.实现一个简易的 javascript debugger，这里说是 debugger 比较标题党，其实只是有点类似于 debugger 的功能，主要利用 js 函数劫持来实现函数的 break point，来看看个简单的例子： 

```
<script type="text/javascript">   
<!--   
var _eval = eval;   
eval = function(s) {   
if (confirm("eval 被调用 \ n\n 调用函数 \ n" + eval.caller + "\n\n 调用参数 \ n" + s)) {   
_eval(s);   
}   
}   
//-->   
</script>   
<html>   
<head>   
</head>   
<body>   
<script type="text/javascript">   
<!--   
function test() {   
var a = "alert(1)";   
eval(a);   
}   
function t() {   
test();   
}   
t();   
//-->   
</script>   
</body>   
</html>  
```

通过 js 函数劫持中断函数执行，并显示参数和函数调用者代码，来看一个完整例子的效果： 

```
>help 
debug commands: 
bp <function name> - set a breakpoint on a function, e.g. "bp window.alert". 
bl - list all breakpoints. 
bc <breakpoint number> - remove a breakpoint by specified number, e.g. "bc 0". 
help - help information. 
>bp window.alert 
* breakpoint on function "window.alert" added successfully. 
>bl 
* 1 breakpoints: 
0 - window.alert 
>bc 0 
* breakpoint on function "window.alert" deleted successfully. 
```

这里演示设置断点，察看断点和删除断点，完整代码在本文附录[1]给出。 

2.设置陷阱实时捕捉跨站测试者，搞跨站的人总习惯用 alert 来确认是否存在跨站，如果你要监控是否有人在测试你的网站 xss 的话，可以在你要监控的页面里 hook alert 函数，记录 alert 调用情况： 

```
<script type="text/javascript">   
<!--   
function log(s) {   
var img = new Image();   
img.style.width = img.style.height = 0;   
img.src = "http://yousite.com/log.php?caller=" + encodeURIComponent(s);   
}   
var _alert = alert;   
window.alert = function(s) {   
log(alert.caller);   
_alert(s);   
}   
//-->   
</script>  
```

当然，你这个函数要加到页面的最开始，而且还要比较隐蔽一些，赫赫，你甚至可以使 alert 不弹框或者弹个警告框，让测试者抓狂一把。 

3.实现 DOM XSS 自动化扫描，目前一般的 XSS 自动化扫描的方法是从 http 返回结果中搜索特征来确定是否存在漏洞，但是这种方法不适用于扫描 DOM XSS，因为 DOM XSS 是由客户端脚本造成的，比如前段时间剑心发现的 google 的跨站 (见附录 [2]) 原理如下：

``` 
document.write(document.location.hash); 
```

这样的跨站无法反映在 http response 里，所以传统扫描方法没法扫描出来。但是如果你从上个例子里受到启发的话，一定会想到设置陷阱的办法，DOM XSS 最终导致 alert 被执行，所以我们 hook alert 函数设置陷阱，如果 XSS 成功则会去调用 alert 函数，触发我们的陷阱记录结果，这样就可以实现 DOM XSS 的自动化扫描，陷阱代码类似于上面。 

4.灵活的使用 js 劫持辅助你的页面代码分析工作，比如分析网页木马时，经常会有通过 eval 或者 document.write 来进行加密的情况，于是我们编写段 hook eval 和 document.write 的小工具，辅助解密： 

```
<script type="text/javascript">   
<!--   
var _eval = eval;   
eval = window.execScript = window.document.write = window.document.writeln = function(s) {   
document.getElementById("output").value = s;   
}   
//-->   
</script>   
<html>   
<body>   
input:   
<textarea id="input" cols="80" rows="10"></textarea>   
<input type="button" onclick="javascript: _eval(document.getElementById('input').value);" value="decode" />   
<br />   
output:   
<textarea id="output" cols="80" rows="10"></textarea>   
</body>   
</html>  
```

在 input 框里输入加密的代码： 

```
eval(unescape("%61%6C%65%72%74%28%31%29%3B")); 
```

在 output 框里输出解码后的代码： 

```
alert(1); 
```

当然你还能想到更多的灵活应用。