# 浅谈 JavaScript 数据类型  
  
## 数据类型是什么？

我们接触的绝大多数程序语言来说，把数据都进行了分类，包括数字、字符、逻辑真假：int，long，string，boolean....等等；我们都知道计算机对数据处理时是采用二进制的方式。将数据加载到内存中，并且通过 CPU 调度进行计算得到最终结果，那么，难道内存存储数据时会记录所以数据的类型吗？我认为答案是否定的，内存中的数据应该会根据所占内存的大小来进行区分和计算的，两种不同类型数据的计算，对于 CPU 来说只是调度了两个所占内存大小不一的数据来进行计算，所以对于 CPU 来说，数据只有 1 和 0。那么这里就出现了问题，有些人会说 Java 语言某两种数据不能直接计算，必须转换才能计算。这里，就是强类型和弱类型的区别，强类型语言会对每一种数据进行严格的检查，也就是对于每种类型内存所占空间进行检查，如果不符合要求，就不允许编译或者运行。弱类型则没有对数据进行严格的检查，允许大多数数据类型直接进行计算，JavaScript 属于弱类型。

## JavaScript 有哪些类型？

包括以下几种：

Number:也就是数字包括浮点数

Boolean：真假（true or false）

String:字符串

Null：空对象指针，表明指向的内存空间不存在

Undefined：未定义，表明指向的内存空间存在，但是没有数据

Object：一中复杂的数据类型，如果熟悉类似 Java 面向对象语言，对此应该很好理解
通过以上这 6 中类型，就能将数据进行分类了，对于数据的容器 JavaScript 统一用关键字 var 声明，那么如何确定一个变量是那种类型呢？这就要用到关键字 typeof

这里，需要说明的是 typeof 是一个操作符(类似+、-、*、/) 而非 function 你可以直接 typeof a 使用（尽管这样不推荐）。而 null 和 undefined 在比较大小时是相等的。因为 undefined 派生自 null。

下边是 typeof 的举例
  
```
[js] view plaincopy
var mesage='some string';  
  
 var obj=new Object();  
  
 var a;  
  
 alert(typeof message);//'string'  
  
 alert(typeof(message));//'string'  
  
 alert(typeof(95));//'number'  
  
 alert(typeof(a));//'undefined'  
  
 alert(typeof(null==undefined));//'boolean'  
  
 alert(null==undefined);//'true'  
  
 alert(obj);//'object'  
  
 alert(null);//'object'(在不同浏览器中也可能为'null')  
```  

以上就是关于 javascript 数据类型的所有内容了，希望大家能够喜欢。