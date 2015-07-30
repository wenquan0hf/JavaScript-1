# 浅谈 javascript 的数据类型检测

## javascript 的数据 

javascript 的数据分为两种：简单数据和复杂数据。简单数据包含 number，string，boolean，undefined 和 null 这五种；复杂数据只有一种即 object。【此处友情鸣谢李战老师，<<悟透 JavaScript>> 写得太传神，印象太深刻了】 

## javascript 的数据类型检测 

### 万能的 typeof 

我们先测试一下通过 typeof 来获取简单数据类型。什么也别说了，上代码是王道： 

```
// 获取变量 obj 的数据类型   
function getType(obj) {   
return typeof (obj);   
}   
/* 常量获取类型 */   
alert(getType(1)); //number   
alert(getType("jeff wong")); //string   
alert(getType(true)); //boolean   
alert(getType(undefined)); //undefined   
alert(getType(null)); //object   
/* 变量获取类型 */   
var num = 1;   
var str = "jeff wong";   
var flag = true;   
var hell = undefined;   
var none = null;   
alert(getType(num)); //number   
alert(getType(str)); //string   
alert(getType(flag)); //boolean   
alert(getType(hell)); //undefined   
alert(getType(none)); //object  
```

正如你所看到的那样，通过 typeof 运算符，前面四个简单数据类型完全在意料之中，但是 typeof null 却返回 object。应该注意到，null 是 null 类型的唯一值，但 null 并不是 object，具有 null 值的变量也并非 object，所以直接通过 typeof，并不能正确得到 null 类型。要正确获取简单数据类型，只要在 getType 的地方加点改进就可以了： 

```
function getType(obj) {   
return (obj === null) ? "null" : typeof (obj);   
}  
```

接着来试一下复杂数据类型 object： 

```
 <br>function Cat() { <br>} <br>Cat.prototype.CatchMouse = function () { <br>//do some thing <br>} <br>// 获取变量 obj 的数据类型 function getType(obj) {   
return (obj === null) ? "null" : typeof (obj);   
}var obj = new Object(); <br>alert(getType(obj)); //object <br>var func = new Function(); <br>alert(getType(func)); //function <br>var str = new String("jeff wong"); <br>alert(getType(str)); //object <br>var num = new Number(10); <br>alert(getType(num)); //object <br>var time = new Date(); <br>alert(getType(time)); //object <br>var arr = new Array(); <br>alert(getType(arr)); //object <br>var reg = new RegExp(); <br>alert(getType(reg)); //object <br>var garfield = new Cat(); <br>alert(getType(garfield)); //object <br>  
```

我们看到，除了 Function（请注意大小写）返回了 function，不管是 javascript 的常见内置对象 Object，String 或者 Date 等等，还是自定义 function，通过 typeof 返回的无一例外，通通都是 object。但是对于自定义 function，我们更愿意得到它的 “庐山真面目”（示例中即 Cat，而非 object），而显然，typeof 不具备这种转换处理能力。 

### constructor，想大声说爱你 

既然万能的 typeof 也有无解的时候，那么我们怎么判断一个变量是否是自定义的 function 实例呢？我们知道，javascript 的所有对象都有一个 constructor 属性，这个属性可以帮我们判断 object 数据类型，尤其是对自定义 function 同样适用： 

```
var obj = "jeff wong";   
alert(obj.constructor == String); //true   
obj = new Cat();   
alert(obj.constructor == Cat); //true  
```

但是，下面的代码您也可以测试一下： 

```
//alert(1.constructor); // 数字常量 出错 数字常量无 constructor   
var num = 1;   
alert(num.constructor == Number); //true   
alert("jeff wong".constructor == String); //true   
var str = "jeff wong";   
alert(str.constructor == String); //true   
var obj= null;   
alert(obj.constructor); //null 没有 constructor 属性   
none = undefined;   
alert(obj.constructor); //undefined 没有 constructor 属性  
```

实验证明，数字型常量，null 和 undefined 都没有 constructor 属性。 

到这里，您会和我一样庆幸认为终于大功告成了吗？下面的代码或许还能有点启发和挖掘作用： 

```
function Animal() {   
}   
function Cat() {   
}   
Cat.prototype = new Animal();   
Cat.prototype.CatchMouse = function () {   
//do some thing   
}   
var obj = new Cat();   
alert(obj.constructor == Cat); //false ？？   
alert(obj.constructor == Animal); //true 理解  
```

原来对于原型链继承的情况，constuctor 也不那么好使了。那怎么办？
 
### 直观的 instanceof 

嘿嘿，有请 instanceof 隆重登场。看它的命名，好像是获取某一个对象的实例，也不知这样理解对不对？不管怎样，我们还是动手改进上面的代码测试一下先： 

```
function Animal() {   
}   
function Cat() {   
}   
Cat.prototype = new Animal();   
Cat.prototype.CatchMouse = function () {   
//do some thing   
}   
var garfield = new Cat();   
alert(garfield instanceof Cat); //true 毫无疑问   
alert(garfield instanceof Animal); //true 可以理解  
```