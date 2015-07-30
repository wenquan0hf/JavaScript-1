# 浅谈 JavaScript 字符串与数组  
  
## JavaScript 字符串  

字符串是一系列字符的集合，包括英文字母、标点符号、特殊符号、汉字等。

在 JavaScript 中，字符串可以使用双引号（" "）或单引号（' '）来表示。

双引号和单引号必须成对出现，双引号里面可以包含单引号，单引号里面也可以包含双引号。

例如：
  
```
[js] view plaincopy
var myStr1=" My name is ' xiaohua ' ! ";  
  
 var myStr2=' " This is my dream ! " , Tom said . ' ;  
```

字符串的长度通过 length 来获取，例如：
  
```
[js] view plaincopy
myStr1.length;  
  
 myStr2.length;  
```

## JavaScript 数组

数组用来在单独的变量中存储一系列的值。

在 JavaScript 中，可以通过以下几种方法来定义数组。

## 使用关键词 new 来创建数组对象

例如，创建一个名为 myArray 的数组并赋值：
  
```
[js] view plaincopy
var myArray=new Array();  
  
 myArray[0] = " zhangming ";  
  
 myArray[1] = " zhaowei ";  
  
 myArray[2] = " wanghua ";  
```

也可以在创建对象的同时赋值：
  
```
[js] view plaincopy
var myArray=new Array(" zhangming " , " zhaowei " , " wanghua ");  
```

## 使用 [ ] 直接创建数组

例如，创建一个名为 myArray 的数组并赋值：
  
```
[js] view plaincopy
var myArray=[];  
  
 myArray[0] = " zhangming ";  
  
 myArray[1] = " zhaowei ";  
  
 myArray[2] = " wanghua ";  
```

当然，也可以在创建数组的同时进行赋值：
  
```
[js] view plaincopy
var myArray=[ " zhangming " , " zhaowei " , " wanghua " ];  
```

## 创建 键/值 对 数组

例如，创建一个名为 myArray 的数组并赋值：
  
```
[js] view plaincopy
var myArray=new Array(); // 也可以使用 var myArray=[ ];  
  
 myArray["zhangming"] = " 22 ";  
  
 myArray["zhaowei"] = " 21 ";  
  
 myArray["wanghua"] = " 30 ";  
```

## 修改数组

数组在创建和赋值后是可以修改的，例如：
  
```
[js] view plaincopy
myArray[0] = " zhangming_1 ";  
  
 myArray[1] = " zhaowei_1 ";  
  
 myArray["zhangming"] = " 42 ";  
  
 myArray["zhaowei"] = " 61 ";  
```

## 数组长度

在 JavaScript 中，通过 length 来获得数组长度，例如：
  
```
[js] view plaincopy
myArray.length  
```

以上所述就是本文的全部内容了，希望大家能够喜欢。