# 浅谈 Javascript 面向对象编程


## 数据类型与包装类 

<table>
<tr>
<td><strong>包装类</strong></td>
<td><strong>类型名</strong></td>
<td><strong>常见值</strong></td>
<td><strong>分类</strong></td>
</tr>
<tr>
<td>Number</td>
<td>number</td>
<td>123.123</td>
<td>基本数据类型</td>
</tr>
<tr>
<td>Boolean</td>
<td>Boolean</td>
<td>true、false</td>
<td>基本数据类型</td>
</tr>
<tr>
<td>String</td>
<td>string</td>
<td>“hello world!”</td>
<td>基本数据类型</td>
</tr>
<tr>
<td>Object</td>
<td>object</td>
<td>{}、[]</td>
<td>复合数据类型</td>
</tr>
<tr>
<td>Function</td>
<td>function</td>
<td>function(){}</td>
<td>特殊类型</td>
</tr>
<tr>
<td>无</td>
<td>undefined</td>
<td>undefined、未定义</td>
<td>小数据类型</td>
</tr>
<tr>
<td>无</td>
<td>null</td>
<td>null</td>
<td>小数据类型</td>
</tr>
</table>

内置类型与本文关系不大，不列出。 

## 引用类型与值类型 

引用类型：object function 

值类型：number、boolean、string、null、undefined 

## new function（构造器）与 prototype（原型） 

关于 prototype 的设计模式就不多说了，网上很多介绍，以一个例子介绍一下 js 中使用 new 构造对象的过程。 

```
function classname(){this.id=0;} var v=new classname(); 
```

当使用 function 构造对象时，进行以下流程： 

1. 查找 classname 的 prototype，并进行浅拷贝。 
2. 绑定 this 指针到拷贝来的对象。 
3. 将 this.constructor 属性设置为 classname。   
[注：其实 classname.prototype.constructor 的值也被设置为 classname，第六部分会说明] 
4. 执行用户 {} 中的代码。 
5. 返回 this 指针赋予左值 v。 

## 实现面向对象的三个基本特征 

### 封装 

封装这个大家都明白，在 js 中，重点在于访问权限。在其他原生支持面向对象语言中，一般支持 public、protected、private 三个关键字来控制访问权限，但在 js 中，我们只能依靠复杂的作用域关系来控制： 

```
function classname(a){   
  
var uid=a; //uin 为模拟 private，作用域为 {}，外部无法使用   
  
this.getuid=function(){return a;} // 为 uid 提供一个外部只读接口 obj.getuid();   
  
this.setuid=function(val){a=val} // 为 uid 提供一个外部可写接口 obj.setuid(5);   
  
this.id=uid; //id 为模拟 public obj.id 使用   
  
}   
  
classname.prototype.func=function(){}; // 模拟 public 方法 obj.func() 调用   
  
classname.stafunc=function(){}; // 模拟静态方法 classname.stafunc() 调用   
  
var obj=new classname(1);  
```

[!] 非常需要注意的就是，因为 function 是引用类型， classname.prototype.func 是所有对象共享的一个 function 对象（每个对象仅存着引用），因此对象规模不大。而使用 this.getuid 和 this.setuid 为定义一个 function，因此每个对象实例都会存一份，如果放肆使用这种方法，会造成对象规模庞大，影响性能。个人认为模拟 private 变量的意义不大。 

[!] 如果有需求真的需要大量使用 this.xxx=function(){} 这种情况，在 function(){} 中的 this 指针与最外的 this 指针是不同的，最好在类定义的首行加上 var _this=this;，这样在 this.xxx=function(){} 中也可以方便使用绑定的指针。 

### 继承 

继承的实现，主要有 2 种方法：第一种是使用 javascript 本身的原型模型，通过给 prototype 赋值并改变其 constructor 属性来实现继承；第二种方法是不使用 prototype，手动实现将父对象的所有属性方法深拷贝到子对象。比如 A 需要继承 B，第一种写法可以：A.prototype=new B();A.prototype.constructor=A; 第二种写法可以写一个递归，或者使用 jquery 中的方法 extend。另外，如果要实现多继承的话，prototype 就真的好麻烦了（需要依次多个类，还要建空对象来接），第二种方法就比较简单，依次拷贝即可。一般这种继承为了找父类方便，可以在对象中加个属性，引用父类。 

### 多态 

函数重载就不说了，都会，检查参数即可，很灵活。隐藏属性就是直接赋值 undefined。需要注意的是，如果是打算继承 B 类的 prototype，一定要建一个空对象来接，否则的话，你给类写方法的话，相当于直接修改了 prototype，就算不写方法，你最后修改 constructor 时也会造成继承链错乱，接个空对象很容易： 

```
function temp(){};   
  
temp.prototype=B;   
  
var obj=new temp();  
```

这样再让需要继承 B.prototype 的类继承 obj 即可，即便修改 prototype 也不会影响到 B。而且也不像继承 new B() 那样浪费很多空间。 

## 深拷贝与浅拷贝 

这个和其他语言中没什么区别，浅拷贝就是直接拷贝，遇到引用类型或类类型不再深入。深拷贝则是根据类型判断，进行递归拷贝。 

## prototype.constructor 

这个值主要是用于维护继承的原型链。一篇文章已经写的非常详细，请参考：http://bbs.51js.com/thread-84148-1-1.html 

## JS 的面向对象开发 

由于我不是前台开发人员，见过项目有限，仅谈自己的经验。 

我开发过的 B/S，常用两种架构，一种是以 CGI 为主，由后台语言去生成 HTML，JS 仅仅做一些用户交互，ajax 通信等。另外一种是使用 MVC，后台语言仅仅生成 JSON，View 层完全由 JS 组件在客户端实现。后者一般大量使用面向对象的思想进行编程，将组件封装成类，将 JSON 传入构造函数，再由控制器或布局组件 Add 进来。由于组件可以重用，在开发后台管理系统、JS 游戏上，效率还是很可观的。
