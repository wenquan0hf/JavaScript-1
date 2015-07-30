# 浅谈 JavaScript 函数参数的可修改性问题

一道笔试题思考而来的，通常情况下没人会在函数内部修改参数值。这里仅拿出来讨论，有三种方式可以修改。

### 直接修改函数声明时的形参

```
function f1(a) {   
    alert(a);   
    a = 1;// 修改形参 a   
    alert(1 === a);   
    alert(1 === arguments[0]);   
}   
f1(10);  
```

函数 f1 定义了参数 a，调用时传参数 10，先弹出 10，修改 a 为 1，弹出两次 true，a 和 arguments[0]都为 1 了。

### 通过函数内部的 arguments 对象修改

```
function f2(a) {   
    alert(a);   
    arguments[0] = 1;// 修改 arguments   
    alert(1 === a);   
    alert(1 === arguments[0]);   
  
}  
```

效果同函数 f1。

### 函数内部声明的局部变量与形参同名

```
function f3(a) {   
    alert(a);   
    var a = 1;// 声明局部变量 a 且赋值为 1   
    alert(1 === a);   
    alert(arguments[0]);   
}   
f3(10);  
```

函数 f3 定义了形参 a，函数内部声明局部变量 a 同时赋值为 1，但这里的 a 仍然是参数 a，从最后弹出的 arguments[0]被修改为 1 可以证明。

### 如果只是声明局部变量 a，却不赋值，情况又不一样了

```
function f3(a) {   
    var a;// 仅声明，不赋值   
    alert(a);   
    alert(arguments[0]);   
}   
f3(10);  
```