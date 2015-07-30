# 浅谈 JavaScript Math 和 Number 对象  
  
## Math 对象

### 介绍

　　Math 对象，是数学对象，提供对数据的数学计算，如：获取绝对值、向上取整等。无构造函数，无法被初始化，只提供静态属性和方法。  

### 构造函数

　　无 ：Math 对象无构造函数，无法被初始化，只提供静态属性和方法。  

### 静态属性

#### Math.E ：常量 e 。返回自然对数的底数：2.718281828459045

#### Math.PI ：常量 π。返回圆周率的值 ：3.141592653589793

### 静态方法  

#### Math.sin(value) ：正弦函数

#### Math.cos(value) ：余弦函数

#### Math.tan(value) ：正切函数

#### Math.asin(value) ：反正弦函数

#### Math.acos(value) ：反余弦函数

#### Math.atan(value) ：反正切函数

#### Math.abs(value) ：返回绝对值  

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Number} 返回参数的绝对值数字。若参数不为数字，返回NaN。

示例：  
  
```
[js] view plaincopy
h.abs('123'); // => 123 ：纯数字字符串  
  
Math.abs('-123'); // => 123  
  
Math.abs(123); // => 123  
  
Math.abs(-123); // => 123  
  
Math.abs('123a'); // => NaN ：非纯数字字符串  
```  

#### Math.ceil(value) ： 对一个数向上取整，并不是四舍五入

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Number} 返回取整后的值。若参数不为数字，返回 NaN。

示例：
  
```
[js] view plaincopy
Math.ceil(2.7); // => 3  
  
Math.ceil(2.3); // => 3 ：2.3 向上取整返回 3  
  
Math.ceil(-2.7); // => -2  
  
Math.ceil(-2.3); // => -2  
  
Math.ceil('2.7'); // => 3 ：纯数字字符串  
  
Math.ceil('2.7a'); // => NaN ：非纯数字字符串  
```  

#### Math.floor(value) ：对一个数向下取整，并不是四舍五入

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Number} 返回取整后的值。若参数不为数字，返回 NaN。

示例：  
  
```
[js] view plaincopy
Math.floor(2.7); // => 2  
  
Math.floor(2.3); // => 2  
  
Math.floor(-2.7); // => -3 ：-2.7 向下取整返回 -3  
  
Math.floor(-2.3); // => -3  
  
Math.floor('2.7'); // => 2 ：纯数字字符串  
  
Math.floor('2.7a'); // => NaN ：非纯数字字符串  
```   

#### Math.max(value1,value2...valueN) ：返回参数中最大的值

参数：

①value1,value2.....valueN {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Number} 返回最大值。若一个参数不为数字，返回 NaN。

示例：

```
[js] view plaincopy
Math.max(1, 2, 3, 4, 5); // => 5  
  
Math.max(1, 2, 3, 4, '5' ); // => 5  
  
Math.max(1, 2, 3, 4, 'a'); // => NaN  
```　

#### Math.min(value1,value2...valueN) ：返回参数中最小的值

参数：

①value1,value2.....valueN {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Number} 返回最大值。若一个参数不为数字，返回 NaN。

示例：

```
[js] view plaincopy
Math.min(1, 2, 3, 4, 5); // => 1  
  
Math.min('1', 2, 3, 4, 5); // => 1  
  
Math.min(1, 2, 3, 4, 'a'); // => NaN  
```  

#### Math.pow(x,y) ：返回 x 的 y 次方

参数：

①x {Number | NumberStr} ：数字或者纯数字的字符串。

②y {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Number} 返回x的y次方。若一个参数不为数字，返回 NaN。

示例：  
  
```
[js] view plaincopy
Math.pow(2, 3); // => 8 ：2的3次方  
  
Math.pow(3, 2); // => 9 ：3的2次方  
  
Math.pow('4', 2); // => 16 ：4的2次方  
  
Math.pow('2a', 2); // => NaN  
```  

#### Math.random() ：返回一个伪随机数,大于0，小于1.0

参数：无

返回值：

{Number} 返回一个伪随机数,大于0，小于1.0

示例：  
  
```
[js] view plaincopy
Math.random(); // => 0.8982374747283757  
  
Math.random(); // => 0.39617531932890415  
  
Math.random(); // => 0.35413061641156673  
  
Math.random(); // => 0.054441051790490746  
```    

#### Math.round(value) ： 四舍五入后取整

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串。

返回值：

{Integer} 返回参数四舍五入后的整数。若参数不为数字，返回 NaN。

示例：    

```
[js] view plaincopy
Math.round(2.5); // => 3  
  
Math.round(2.4); // => 2  
  
Math.round(-2.6); // => -3  
  
Math.round(-2.5); // => -2 ：-2.5四舍五入为 -2  
  
Math.round(-2.4); // => -2  
  
Math.round('2.7'); // => 3 ：纯数字字符串  
  
Math.round('2.7a'); // => NaN ：非纯数字字符串  
```    

#### Math.sqrt(value) ：返回参数的平方根

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串

返回值：

{Number} 返回参数的平方根

示例：
  
```
[js] view plaincopy
console.log( Math.sqrt(9) ); // => 3  
  
console.log( Math.sqrt(16) ); // => 4  
  
console.log( Math.sqrt('25') ); // => 5  
  
console.log( Math.sqrt('a') ); // => NaN  
```  

## Number 对象  

### 介绍

　　Number 对象，是数字对象，包含js中的整数、浮点数等等。  

### 定义
  
```
[js] view plaincopy
var a = 1;  
  
var b = 1.1;  
```  

### 静态属性

#### Number.MAX\_VALUE ：表示 JS 中最大的数字，约为 1.79e+308

#### Number.MIN\_VALUE ：表示 JS 中最小的数字，约为 5e-324

#### Number.NaN ：返回 NaN，表示非数字值，与任意其他数字不等，也包括 NaN 本身。应使用 Number.isNaN() 来进行判断。

#### Number.NEGATIVE\_INFINITY ：返回 -Infinity ，表示负无穷。

#### Number.POSITIVE\_INFINITY ：返回 Infinity ，表示正无穷。进行计算的值大于Number.MAX\_VALUE 就返回 Infinity 。

### 静态方法

#### Number.isInteger(value) ：判断参数是否为整数 

参数：

①value {Number} ：数字

返回值：

{Boolean} 返回参数是否为整数 。纯整数的字符串也返回 false。

示例：

```
[js] view plaincopy
Number.isInteger(1); // => true  
  
Number.isInteger(1.1); // => false  
  
Number.isInteger('1'); // => false ：纯整数的字符串也返回false  
  
Number.isInteger('1.1'); // => false  
  
Number.isInteger('a'); // => false ：非字符串返回false  
```

#### Number.isNaN(value) ：判断参数是否为 NaN

参数：

①value {Object} ：任意类型

返回值：

{Boolean} 返回参数是否为 NaN 。

示例：

```
[js] view plaincopy
Number.isNaN(NaN); // => true  
  
Number.isNaN('NaN'); // => false :'NaN'字符串，并不为NaN  
  
Number.isNaN(1); // => false  
  
Number.isNaN('1'); // => false  
```

#### Number.parseFloat(value) ：把参数转换为浮点数

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串

返回值：

{Integer | Float} 返回整数或浮点数数值

示例：

```
[js] view plaincopy
Number.parseFloat(1); // => 1 ：整数还是返回整数  
  
Number.parseFloat(1.1); // => 1.1  
  
Number.parseFloat('1aaa'); // => 1 ：字符串前面为数字的，只返回数字  
  
Number.parseFloat('1.1aaa'); // => 1.1  
  
Number.parseFloat('a1'); // => NaN ：非数字开头，返回NaN  
  
Number.parseFloat('a'); // => NaN  
```

#### Number.parseInt(value) ：把参数转换为整数

参数：

①value {Number | NumberStr} ：数字或者纯数字的字符串

返回值：

{Integer} 返回整数数值

示例：
  
```
[js] view plaincopy
Number.parseInt(1); // => 1  
  
Number.parseInt(1.1); // => 1 ：浮点数返回整数  
  
Number.parseInt('1aaa'); // => 1 ：字符串前面为数字的，只返回数字  
  
Number.parseInt('1.1aaa'); // => 1  
  
Number.parseInt('a1'); // => NaN ：非数字开头，返回NaN  
  
Number.parseInt('a'); // => NaN  
```  

### 实例方法

#### toExponential(value) ：将一个数字转为指数类型，参数表示小数点后的位数

参数：

①value {Number} ：表示小数点后的位数

返回值：

{String} 返回转换后的指数类型字符串

示例：

```
[js] view plaincopy
(123456789).toExponential(2); // => 1.23e+8 ：小数点2位  
  
(123456789).toExponential(5); // => 1.23457e+8 ：小数点5位  
  
(123456789).toExponential(10); // => 1.2345678900e+8 ：小数点10位，不足位数用0补位  
```

#### toFixed(value) ：将一个数字转换为指定小数位数的字符串。不传入参数，就是没小数位。返回值为四舍五入

参数：

①value {Number} ：表示小数点后的位数

返回值：

{String} 返回转换后的字符串；不够小数位以 0 填充；返回值为四舍五入后的值

示例：

```
[js] view plaincopy
console.log((1).toFixed(2)); // => 1.00  
  
console.log((1.2).toFixed(2)); // => 1.20 ：不足位数，以0补位  
  
console.log((1.277).toFixed(2)); // => 1.28 ：进行了四舍五入  
```

#### toString() ：使用指定的进制，将一个数字转换为字符串。不传入参数，默认为十进制。

参数：

①value {Number} ：表示进制数，取值范围：2到36

返回值：

{String} 转换后进制的字符串

示例：

```
[js] view plaincopy
(10).toString(); // => 10 ：默认为十进制  
  
(10).toString(2); // => 1010 ：二进制  
  
(10).toString(10); // => 10 ：十进制  
  
(10).toString(16); // => a ：十六进制  
```  

### 应用场景

#### 浮点数的加减乘除异常

说明：Js 中的 2 个浮点数进行加减乘除运算，会返回异常的数值，如：0.2+0.7，返回0.899999999999。可以使用 toFixed() 方法，指定小数位。

示例：

```
[js] view plaincopy
console.log(0.2 + 0.7); // => 0.8999999999999999  
  
console.log(0.7 - 0.5); // => 0.19999999999999996  
  
console.log(3.03 * 10); // => 30.299999999999997  
  
// 使用toFixed()方法  
  
console.log( (0.2 + 0.7).toFixed(2) ); // => 0.90  
  
console.log( (0.7 - 0.5).toFixed(2) ); // => 0.20   
  
console.log( (3.03 * 10).toFixed(2) ); // => 30.30  
```

#### 减法运算

说明：Js中进行减法运算时，会先把前后的值转换为数值再进行运算。若转换失败，返回NaN。

示例：

```
[js] view plaincopy
console.log('1' - 0); // => 1 ：纯数字字符串减去0，可以快速转换为Nubmer对象  
  
console.log( ('1' - 0).toFixed(2) ); // => 1.00 ：快速转换为Nubmer对象后调用实例方法  
  
console.log('1' - 'a'); // => NaN ：一方无法转换为Nubmer对象   
```