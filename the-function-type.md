# 浅谈 JavaScript function 函数种类  
  
本篇主要介绍普通函数、匿名函数、闭包函数

## 普通函数介绍

### 示例  
  
```
[js] view plaincopy
function ShowName(name) {  
  
    alert(name);  
  
}  
```  

### Js 中同名函数的覆盖

在 Js 中函数是没有重载，定义相同函数名、不同参数签名的函数，后面的函数会覆盖前面的函数。调用时，只会调用后面的函数。
  
```
[js] view plaincopy
var n1 = 1;  
  
   
  
function add(value1) {  
  
    return n1 + 1;  
  
}  
  
alert(add(n1));//调用的是下面的函数，输出：3  
  
   
  
function add(value1, value2) {  
  
    return value1 + 2;  
  
}  
  
alert(add(n1));//输出：3  
```  

### arguments 对象

arguments 类似于 C# 的 params，操作可变参数：传入函数的参数数量大于定义时的参数数量。
  
```
[js] view plaincopy
function showNames(name) {  
  
    alert(name);//张三  
  
    for (var i = 0; i < arguments.length; i++) {  
  
        alert(arguments[i]);//张三、李四、王五  
  
    }  
  
}  
  
showNames('张三','李四','王五');  
```  

### 函数的默认范围值

若函数没有指明返回值，默认返回的是 'undefined'
  
```
[js] view plaincopy
function showMsg() {  
  
}  
  
alert(showMsg());//输出：undefined  
```  
　　
## 匿名函数   

### 变量匿名函数

#### 说明

可以把函数赋值给变量、事件。  

#### 示例
  
```
[js] view plaincopy
//变量匿名函数，左侧可以为变量、事件等  
  
var anonymousNormal = function (p1, p2) {  
  
    alert(p1+p2);  
  
}  
  
anonymousNormal(3,6);//输出9  
```  

#### 适用场景

①避免函数名污染。若先声明个带名称的函数，再赋值给变量或事件，就造成了函数名的滥用。  

### 无名称匿名函数

#### 说明

即在函数声明时，在后面紧跟参数。Js 语法解析此函数时，里面代码立即执行。   

#### 示例
  
```
[js] view plaincopy
(function (p1) {  
  
    alert(p1);  
  
})(1);  
```  

#### 适用场景  

①只需执行一次的。如浏览器加载完，只需要执行一次且后面不执行的功能。  

##  闭包函数  

### 说明

假设，函数A内部声明了个函数B，函数B引用了函数 B 之外的变量，并且函数 A 的返回值为函数B 的引用。那么函数B就是闭包函数。

### 示例

#### 示例1：全局引用与局部引用
  
```
[js] view plaincopy
function funA() {  
  
    var i = 0;  
  
    function funB() { //闭包函数funB  
  
        i++;  
  
        alert(i)  
  
    }  
  
    return funB;  
  
}  
  
var allShowA = funA(); //全局变量引用：累加输出1,2,3,4等  
  
   
  
function partShowA() {  
  
    var showa = funA();//局部变量引用：只输出1  
  
    showa();  
  
}  
```  

allShowA 是个全局变量，引用了函数 funA。重复运行 allShowA()，会输出 1,2,3,4 等累加的值。

执行函数 partShowA(),因为内部只声明了局部变量 showa 来引用 funA，执行完毕后因作用域的关系，释放 showa 占用的资源。

闭包的关键就在于作用域：全局变量占有的资源只有当页面变换或浏览器关闭后才会释放。var allShowA = funA() 时，相当于 allShowA 引用了 funB()，从而使 funB() 里的资源不被GC 回收，因此 funA() 里的资源也不会。

#### 示例2：有参闭包函数
  
```
[js] view plaincopy
function funA(arg1,arg2) {  
  
    var i = 0;  
  
    function funB(step) {  
  
        i = i + step;  
  
        alert(i)  
  
    }  
  
    return funB;  
  
}  
  
var allShowA = funA(2, 3); //调用的是funA arg1=2，arg2=3  
  
allShowA(1);//调用的是funB step=1,输出 1  
  
allShowA(3);//调用的是funB setp=3,输出 4  
```  

#### 示例3：父函数 funA 内的变量共享
   
```
[js] view plaincopy
function funA() {  
  
    var i = 0;  
  
   function funB() {  
  
        i++;  
  
        alert(i)  
  
    }  
  
    allShowC = function () {// allShowC引用匿名函数,与funB共享变量i  
  
        i++;  
  
        alert(i)  
  
    }  
  
    return funB;  
  
}  
  
var allShowA = funA();  
  
var allShowB = funA();//allShowB引用了funA，allShowC在内部重新进行了绑定，与allShowB共享变量i  
```  

### 适用场景

①保证函数 funA 内里的变量安全，因为外部不能直接访问 funA 的变量。

小伙伴们是否对 javascript 的 function 函数有所了解了呢，有疑问就给我留言吧。