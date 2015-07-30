# 根据一段代码浅谈 Javascript 闭包

```
function f1(){   
var n = 999;   
nAdd = function(){ n += 1;}   
function f2(){   
alert(n);   
}   
return f2;   
}  
```

这里的闭包是 f1，封闭了一个变量 n 和一个函数 f2。 

我们先无视 nAdd，尽量保持原貌重写一下这个函数。 

```
function f1(){   
var n = 999;   
var f2 = function(){ alert(n); };   
return f2;   
}   
var result = f1();   
result();  
```

js 中各个变量以 function 为单元进行封装，当在 function 内部找不到某一变量时，function 会向其所在的上一单元（上下文）中进行查找，一直查找到顶层的 window 域。 

这时就出现一个疑问：这个查找过程是以函数引用位置为起点还是函数体定义的位置为起点？ 

在上面这一段代码中，result 所在域是 window，但是实际的输出结果是 f1 内部的 n 值，所以可以得出结论：变量查找的起点是函数体定义的位置。 

现在再回过头来看 nAdd（第一段代码）。如我们所知，没有关键字 var 定义的变量默认进入 window 域，所以 nAdd 实际为 window.nAdd。这就等同于如下代码： 

```
var nAdd;   
function f1(){   
var n = 999;   
nAdd = function(){ n += 1;}   
function f2(){   
alert(n);   
}   
return function(){ alert(n); };   
}  
```

那么根据我们对 result 的分析，nAdd 的执行将影响 f1 中 n 的值。 

所以有： 

```
function f1(){   
var n = 999;   
nAdd = function(){ n += 1;}   
function f2(){   
alert(n);   
}   
return function(){ alert(n); };   
}   
var result = f1();   
result();   
nAdd();   
result();  
```

这段代码执行最终的输出结果为 1000。 

再看这种情况： 

```
function f1(){   
var n = 999;   
nAdd = function(){ n += 1;}   
function f2(){   
alert(n);   
}   
return function(){ alert(n); };   
}   
  
f1()(); //<--p1   
nAdd();   
f1()(); //<--p2  
```

简述一下执行过程： 

p1 位置，f1 封装了一个匿名的闭包 A，在返回 A 闭包中的函数 A:f2 后继而执行 A:f2，A:f2 输出变量 A:n，结果是 999。 

与此同时，nAdd 被赋值为 A 闭包中的一个函数，下一行执行 nAdd 即让 A:n 的值 + 1。 

p2 位置，f1 封装匿名的闭包 B，所进行的操作都是针对闭包 B 的，随后执行 B:f2 输出的是 B:n，所以最后的结果依然是 999。 

A 和 B 是两个独立的 “包”，互不影响。 

改写一下函数的调用部分： 

```
[js] view plaincopy
function f1(){   
var n = 999;   
nAdd = function(){ n += 1;}   
function f2(){   
alert(n);   
}   
return function(){ alert(n); };   
}   
  
var result = f1();   
result();   
nAdd();   
f1()();   
result(); // <--p3  
```

p3 位置不意外地输出了 1000。