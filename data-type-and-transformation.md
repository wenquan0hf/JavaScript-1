# 浅谈 JavaScript 数据类型及转换  
  
## JavaScript 数据类型

1.Boolean（布尔）

布尔：（值类型）var b1=true;//布尔类型

2.Number（数字）

数值：（值类型）var n1=3.1415926;//数值类型

n1.toFixed(3);//四舍五入保留3位小数。

3.String（字符串）
  
```
[js] view plaincopy
var s1=‘hello';//字符串类型  
```  

字符串：（值类型，字符串不可变特性）  

4.Undefined（未定义）

undefined 属于值类型，与其他值计算得到的结果不是我们想要的，但与数据库中的 null 稍有区别，比如与数字计算或与字符串计算结果。

Undefined 类型、Null 类型都是只有一个值的数据类型，分别为 undefined 与 null.

5.Null（空对象）

6.Object（对象类型）

Object 是引用类型，其他都是基本数据类型 。

String 也是基本类型，不能为 String 添加动态属性，而引用类型时可以的。

引用类型对象 instanceof 类型，判断某个值是否为某个类型，所有引用类型instanceof Object返回都是true

7.应用类型

对象（object）：（引用类型）
  
```
[js] view plaincopy
var tim=new Date();//对象类型(object)  
  
var names=[‘zs','ls','ww'];//数组也是对象类型(object)  
  
var obj=null;//object  
```  

函数：（引用类型）
  
```
[js] view plaincopy
function fun(){  }  //typeof(fun);//输出结果为function,函数类型  
```  

PS:查看变量的类型用 typeof (变量)

## JavaScript 中的 Null 与 undefined

undefined,表示一个未知状态

声明了但是没有初始化的该变量，变量的值是一个未知状态(undefined)。 （访问不存在的属性或对象 window.xxx）方法没有明确返回值时，返回值是一个 undefined.当对未声明的变量应用 typeof 运算符时，显示为 undefined（*）

null 表示尚未存在的对象,null 是一个有特殊意义的值。

可以为变量赋值为 null，此时变量的值为“已知状态”(不是undefined)，即 null。（用来初始化变量，清除变量内容，释放内存）

undefined==null //结果为 true,但含义不同。

undefined===null //false(*),PS:先判断类型是否一致，然后判断值。===严格等于、!==严格不等于

由于==会将值转换类型后再判断是否相等，有时可能会有意想不到的结果，所以推荐使用===。但注意，有些情况使用==能带来更好的效果。

## 类型转换
  
```
[js] view plaincopy
parseInt(arg)将指定的字符串，转换成整数  
  
parseFloat(arg)将指定的字符串，转换成浮点数  
  
Number(arg)把给定的值(任意类型)转换成数字（可以是整数或浮点数）；转换的是整个值，而不是部分值。如果该字符串不能完全转换为整型，则返回NaN。（Not a Number）  
  
isNaN(arg)，判断arg是否为一个非数字（NaN）,NaN与NaN也不相等。  
  
String(arg)把给定的值(任意类型)转换成字符串；  
  
Boolean(arg)把给定的值(任意类型)转换成 Boolean 型；  
  
(*)eval(codeString)将一段字符串的js代码，计算并执行。  
```   

以上所述就是 javascript 的数据类型和转换方法了，希望大家能够喜欢。