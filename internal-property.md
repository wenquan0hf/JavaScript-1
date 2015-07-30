# 浅谈 javascript 函数内部属性  
  
在函数内部有两个特殊的属性：arguments 和 this。arguments 是一个类数组对象，包含传入的所有参数，

但是这个对象还有一个名叫 callee 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数。
请看经典的阶乘函数例子：
  
```
[js] view plaincopy
function Factorial(num) {  
  
            if (num <= 1) {  
  
                return 1;  
  
            } else {  
  
                return num * Factorial(num - 1);  
  
            }  
  
        }  
  
        function Factorial(num) {  
  
            if (num <= 1) {  
  
                return 1;  
  
            } else {  
  
                return num * arguments.callee(num - 1);  
  
            }  
  
        }  
```   

使用第一种方式是没有错的，但是耦合性太高，不太好，函数名改变之后，内部的函数名也要改变

第二种方式就是低耦合的做法，无论函数名怎么改变都不会影响函数执行。
this 引用的是函数据以执行的环境对象，或者也可以说是 this 值
  
```
[js] view plaincopy
window.color = "red";  
  
        var o = {color: "blue"};  
  
        function sayColor() {  
  
            alert(this.color);  
  
        }  
  
        sayColor();//red  
  
        o.sayColor = sayColor;  
  
        o.sayColor();//blue  
```  

caller 属性,保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为 Null
  
```
[js] view plaincopy
function outer() {  
  
            innter();  
  
        }  
  
        function innter(){  
  
            //alert(innter.caller);//耦合性太高  
  
            alert(arguments.callee.caller);  
  
        }  
  
        outer();  
```  

以上就是 javascript 函数内部属性的全部内容了，希望小伙伴们能够喜欢