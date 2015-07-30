# 浅谈 JavaScript 中面向对象技术的模拟

## 引言 

在 C# 和 Java 语言中，面向对象是以类的方式实现的，特别是继承这个特性，类的方式继承表现出了强大的功能，而且也易于学习。JavaScript 不是纯的面向对象的语言，而是基于对象的语言，对象的继承是以原型函数的形式继承的，很多初学者刚开始接触的时候不太理解，但是 JavaScript 这种以原型函数的形式实现面向对象技术，不仅是可行的，而且还为面向对象技术提供了动态继承的功能，本文主要讨论了 JavaScript 的面向对象技术。 

## 原型对象概述 

每个 JavaScript 对象都有原型对象，对象都继承原型对象的所有属性。一个对象的原型是由创建该对象的构造函数定义的。JavaScript 的所有函数都有一个名为 prototype 的属性，该属性引用了原型对象，该原型对象初始化的时候只有 constructor 属性来引用创建该原型对象的对象。JavaScript 没有 Class 定义类的概念，构造函数就定义了类，并初始化类中的属性，每个类的成员都会从原型对象中继承相同的属性，也就是说，原型对象提供了类的实例共享的属性和方法，这就节约了内存。 

当读取一个对象的属性的时候，JavaScript 会先从对象中查找，如果没有查找到，才会到原型对象中查找该属性（或方法），所以，尤其是对于方法，最好保存到原型对象中以便于共享，并且达到节省内存的目的，而且原型对象还有一个强大的功能，那就是如果通过构造函数实例化一些对象后，再给构造函数的原型对象增加属性和方法，那么它原来实例化的对象实例将会继承这些增加的属性和方法。 

## 对象属性、对象方法、类属性、类方法 

每个对象都会有自己单独的实例属性和实例方法的副本，如果实例化 5 个对象，那么就会有 5 个对象的实例属性和实例方法副本。This 关键字引用它们的实例对象，也就是说，谁操作了实例方法，this 就引用谁；访问了哪个实例对象的属性，this 就引用这个实例对象。 

类方法和类属性只有一个副本，类方法调用的时候必须引用类的名字，例如：Date.setHours(); 

下面用一个程序来表现实例属性、实例方法、类属性、类方法 

```
function Mobile(kind,brand) {   
　    this.kind=kind;// 定义手机的种类，例如 GSM/CDMA   
　    this.brand=brand;// 定义手机的品牌，this 关键字表示用该构造函数实例化之后的对象   
　}   
　   
　/**//*  
　 定义类的第二步是在构造函数的原型对象中定义它的实例方法或其他属性  
　 该对象定义的任何属性都将这个类的所有实例继承。  
　   
　 */   
　 // 拨号，这里只是返回电话号码   
　Mobile.prototype.dial = function(phoneNo) {   
　    return phoneNo;   
　};   
　   
　   
　/**//*  
　 定义类的第三步是定义类方法，常量和其他必要的类属性，作为构造函数自身的属性，而不是构造函数  
　 原型对象的属性，注意，类方法没有使用关键字 this，因为他们只对他们的实际参数进行操作。  
　 */   
　// 开机关机方法   
　Mobile.turnOn=function() {   
　   return "The power of mobile is on";   
　}   
　Mobile.turnOff=function() {   
　   return "The power of mobile is off";   
　}   
```　  

// 类属性，这样他们就可以被用作常量，注意实际上他们并不是只读的 

Mobile.screenColor=64K;// 假设该类手机的屏幕颜色都是 64K 彩屏的 

## 子类化 

JavaScript 支持子类化，只需把子类的原型对象用超类实例化即可，但是应该注意，这样子类化之后就会存在一个问题，由于是用超类实例化子类的原型对象取得的，所以就冲掉了自己本身的由 JavaScript 提供的 constructor 属性，为了确保 constructor 的正确性，需要重新指定一下，子类化的程序例子如下： 

```
/***** 子类化 *****/ 
// 下面是子类构造函数智能型手机 
function SmartPhone(os) 
{ 
this.os=os; 

} 
// 我们将 Mobile 对象作为它的原型 
// 这意味着新类的实例将继承 SmartPhone.prototype, 
// 后者由 Mobile.prototype 继承而来 
//Mobile.prototype 又由 Object.prototype 继承而来 
SmartPhone.prototype=new Mobile(GSM,Nokia); 
// 下面给子类添加一个新方法，发送电子邮件，这里只是返回 Email 地址 
SmartPhone.prototype.sendEmail=function(emailAddress) { 
return this.emailAddress 
} 
// 上面的子类化方法有一点缺陷，由于我们明确把 SmartPhone.prototype 设成了我们所创建的一个对象，所以就覆盖了 JS 提供 
// 的原型对象，而且丢弃了给定的 Constructor 属性。该属性引用的是创建这个对象的构造函数。但是 SmartPhone 对象集成了它的 
// 父类的 constructor，它自己没有这个属性，明确设置着一个属性可以解决这个问题： 
SmartPhone.prototype.constructor=SmartPhone; 
var objSmartPhone=new SmartPhone();// 实例化子类
```