# 浅谈 Javascript 中的 Function 与 Object  
  
## Function

函数就是对象,代表函数的对象就是函数对象。所有的函数对象是被 Function 这个函数对象构造出来的。也就是说，Function 是最顶层的构造器。它构造了系统中所有的对象，包括用户自定义对象，系统内置对象，甚至包括它自已。

## Object

Object 是最顶层的对象，所有的对象都将继承 Object 的原型，你也要知道 Object 也是一个函数对象，所以说 Object 是被 Function 构造出来的。

Function 与 Object 关系图：
  
![](images/103.png)
  
```
[js] view plaincopy
<script type="text/javascript">  
  
var Foo= function(){}  
  
var f1 = new Foo();  
  
console.log(f1.__proto__ === Foo.prototype);  
  
console.log(Foo.prototype.constructor === Foo);  
  
var o1 =new Object();  
  
console.log(o1.__proto__ === Object.prototype);  
  
console.log(Object.prototype.constructor === Object);  
  
console.log(Foo.prototype.__proto__ === Object.prototype);  
  
//Function and Object  
  
console.log(Function.__proto__ === Function.prototype);  
  
console.log(Object.__proto__ === Function.prototype);  
  
console.log(Object.prototype.__proto__);  
  
console.log(Object.__proto__ === Function.prototype);  
  
</script>  
```  

小伙伴们读代码的时候可以参考下图片上的关系图，希望大家喜欢。