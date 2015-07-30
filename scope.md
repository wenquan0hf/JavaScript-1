# 浅谈 javascript 中的作用域

JS 中作用域的概念： 

表示变量或函数起作用的区域，指代了它们在什么样的上下文中执行，亦即上下文执行环境。Javascript 的作用域只有两种：全局作用域和本地作用域，本地作用域是按照函数来区分的。 

首先来看几道题目： 

1.

```
if(true){   
var aa= "bb";   
}   
console.log(aa); //bb   
  
for(var i = 0; i <100; i++){   
//do   
}   
console.log(i); //100  
```

2.

```
var bb = '11111';   
function aa() {   
alert(bb);//undefine   
var bb = 'test';   
alert(bb);//test   
　　　var cc = "test1";   
alert(age);// 语法错误   
}   
aa();  
```

3.

```
var test = '1111111';   
function aa() {   
alert(test);   
}   
  
function bb() {   
var test = '22222222';   
aa();   
}   
  
bb();//alert(1111111);  
```

4.

```
alert(typeof aa); //function   
alert(typeof bb); //undefined function aa() { // 函数定义式   
alert('I am 111111111');   
};   
var bb = function() { // 函数表达式   
}   
alert(typeof bb);//function  
```

5.

```
function aa(){   
var bb = "test";   
cc = "测试";   
alert(bb);   
}   
aa();   
alert(cc);// 测试   
alert(bb);// 语法报错  
```

上面这 5 道题目全部概括了 js 中作用域的问题。

可以总结出这么几个观点：

### 无块级作用域 

从第一题中可以看出来，在{}中执行后，变量并没有被销毁，还是保存在内存中的，因此我们可以访问到的。 

### JavaScript 中的函数运行在它们被定义的作用域里, 而不是它们被执行的作用域里. 

这里提到函数的作用域链这个概念，在 ECMA262 中，是这样的 

任何执行上下文时刻的作用域，都是由作用域链 (scope chain) 来实现。
在一个函数被定义的时候，会将它定义时候的 scope chain 链接到这个函数对象的[[scope]]属性。 
在一个函数对象被调用的时候，会创建一个活动对象 (也就是一个对象，然后对于每一个函数的形参，都命名为该活动对象的命名属性，然后将这个活动对象做为此时的作用域链 (scope chain) 最前端， 并将这个函数对象的 [[scope]] 加入到 scope chain 中。 
所以题目 3 结果是 alert(1111111)；

### JS 会提前处理 function 定义式 和 var 关键字 

如题目 4 开始 alert(bb); //undefine ,alert(age)//语法报错，这两个有什么区别呢，原因就是后面有 var bb =“test”，在初始化的时候提前处理了 var 这个关键字，只是这个开始未赋值 

将代码修改成这样的，可以看出来 

```
var dd = '11111';   
function aa() {   
alert(bb);//undefine   
　　 var bb = 'test';   
alert(bb);//test   
　　　var cc = "test1";   
alert(age);// 语法错误   
}   
aa();   
alert(dd);//11111   
alert(cc);// 语法报错  
```

此处 alert(bb)没有报语法错误，alert(age)报语法错误。 

但是请注意： 

```
<script>   
alert(typeof aa); // 结果: undefined   
</script>   
<script>   
function aa() {   
alert('yupeng');   
}   
</script>  
```

这说明 js 预编译是以段为单元的。题目 4 同理 

### 函数级作用域 


函数里面的定义的变量，在函数执行完后就销毁了，不占有内存区域了。 

所以题目 2 最后的 alert(cc)；语法报错，题目 5 最后到 alert(bb)同理。