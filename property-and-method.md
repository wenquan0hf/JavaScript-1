# 浅谈 javascript 函数属性和方法  
   
每个函数都包含两个属性：length 和 prototype

length:当前函数希望接受的命名参数的个数

prototype：是保存他们所有实力方法的真正所在  
  
```
[js] view plaincopy
function sayName(name) {  
  
            alert(name);  
  
        }  
  
        function sum(num1, num2) {  
  
            return num1 + num2;  
  
        }  
  
        function sayHi() {  
  
            alert("hi");  
  
        }  
  
        alert(sayName.length);//1 参数个数一个  
  
        alert(sum.length);//2 参数个数2个  
  
        alert(sayHi.length);//0 没有参数  
```  

每个函数都包含两个非继承而来的方法：apply() 和 call()

这两个方法都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值

首先 apply() 接受两个参数：一个是函数运行的作用域，另一个参数数组（可以是数组实例也可以是 arguments 对象）  
  
```
[js] view plaincopy
function sum(num1, num2) {  
  
            return num1 + num2;  
  
        }  
  
        function callSum1(num1, num2) {  
  
            return sum.apply(this, arguments);//传入arguments对象  
  
        }  
  
        function callSum2(num1, num2) {  
  
            return sum.apply(this, [num1, num2]);  
  
        }  
  
        alert(callSum1(10, 10));//20  
  
        alert(callSum2(10, 20));//30  
```  

其次，call 方法第一个参数没有变化，变化的是其余的参数都是传递参数，传递给函数的参数需要逐个列举出来
  
```
[js] view plaincopy
function sum(num1, num2) {  
  
            return num1 + num2;  
  
        }  
  
        function callSum(num1, num2) {  
  
            return sum.call(this, num1, num2);  
  
        }  
  
        alert(callSum(10, 200));  
```  

至于使用哪个方法更方便，完全取决于你的意愿。如果没有参数，用哪个都一样。

但是，apply 和 call 方法的出现绝对不是只是为了怎样去船体参数。

它们真正的用武之地在于扩充函数赖以运行的作用域。  
  
```
[js] view plaincopy
window.color = "red";  
  
        var o = {color: "blue"};  
  
        function sayColor() {  
  
            alert(this.color);  
  
        }  
  
        sayColor();//red  
  
        sayColor.call(this);//red  
  
        sayColor.call(window);//red  
  
        sayColor.call(o);//blue  
```  

使用 apply 和 call 来扩充作用域的最大的好处就是不需要与方法有任何的耦合关系。

ECMAScript5 还定义了一个方法：bind()。这个方法会创建一个函数的实例，其 this 值会被绑定到传给 bind 函数的值
  
```
[js] view plaincopy
window.color = "red";  
  
        var o = {color: "blue"};  
  
        function sayColor() {  
  
            alert(this.color);  
  
        }  
  
        var bindFun = sayColor.bind(o);  
  
        bindFun();//blue  
```  

以上就是本文的全部内容，希望小伙伴们能够喜欢。