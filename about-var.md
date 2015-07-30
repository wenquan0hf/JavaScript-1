# 浅谈 JavaScript 中定义变量时有无 var 声明的区别

前段时间回答了一个关于定义变量时使用关键字 var 与否的区别，总结回顾一下。

1.在函数作用域内 加 var 定义的变量是局部变量，不加 var 定义的就成了全局变量。

使用 var 定义：

```
var a = 'hello World';  
function bb(){  
 var a = 'hello Bill';  
 console.log(a);    
}  
bb()      //'hello Bill'  
console.log(a);  //'hello world'  
```

不使用 var 定义：

```
var a = 'hello World';  
function bb(){  
 a = 'hello Bill';  
 console.log(a);    
}  
bb()      //'hello Bill'  
console.log(a);  //'hello Bill'  
```

2.在全局作用域下，使用 var 定义的变量不可以 delete，没有 var 定义的变量可以 delete。也就说明隐含全局变量严格来说不是真正的变量，而是全局对象的属性，因为属性可以通过 delete 删除，而变量不可以。

3.使用 var 定义变量还会提升变量声明，即

使用 var 定义：

```
function hh(){  
 console.log(a);  
 var a = 'hello world';  
}  
hh()      //undefined  
```

不使用 var 定义：

```
function hh(){  
 console.log(a);  
 a = 'hello world';  
}  
hh()      //'a is not defined'  
```