# 浅谈 javascript 面向对象程序设计  
  
ECMA-262 把对象定义为：“无序属性的 集合，其属性可以包含基本值、对象或者函数”

理解对象，最简单的方式就是通过创建一个 Object 的实例，然后为它添加属性和方法
  
```
[js] view plaincopy
var person = new Object();  
  
        person.name = "Xulei";  
  
        person.age = "23";  
  
        person.job = "前端工程师";  
  
        person.sayName = function () {  
  
            alert(this.name);  
  
        }  
```  

还可以这样写
  
```
[js] view plaincopy
var person = {  
  
            name: "xulei",  
  
            age: 23,  
  
            job: "前端工程",  
  
            sayName: function () {  
  
                alert(this.name)  
  
            }  
  
        }  
```  

## 属性类型：数据属性和访问其属性  

### 数据属性，有4个描述其行为的特性

[Configurable]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性，默认值为true

[Enumerable]：表示能否通过 for-in 返回属性，默认值为 true

[Writable]：表示能否修改属性，默认值为 true

[Value]：包含这个属性的数据值。默认值为 undefined  
  
```
[js] view plaincopy
var person = {  
  
            name: "xulei"  
  
        }  
```  

这里创建了一个 person 对象，value 值就是 “xulei”

要修改属性的默认特性，必须使用 ECMAScript5的Object.defineProperty (属性所在的对象,属性的名字,描述符对象)

描述符对象必须是 configurable、enumerable、writable、value  
  
```
[js] view plaincopy
var peron = {}  
  
        Object.defineProperty(peron, "name", {  
  
            writable: false,//属性不能被修改  
  
            value: "徐磊-xulei"  
  
        });  
        alert(peron.name);//徐磊-xulei  
  
        peron.name = "徐磊";  
  
        alert(peron.name);//徐磊-xulei  
```  

以上操作在非严格模式下赋值操作会被忽略，如果在严格模式下会抛出异常

一旦把属性定义为不可配置的就不能把它变回可配置的了。

在多数情况下都没有必要利用 Object.defineProperty() 方法提供的这些高级功能。但是对理解 javascript 非常有用。

建议读者不要在 ie8 上使用此方法。  

### 访问其属性，有4个特性

[Configurable]:表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性，默认值为 true

[Enumerable]:表示能否通过 for-in 返回属性，默认值为 true

[Get]:在读取时调用的函数

[Set]:在写入属性时调用的函数