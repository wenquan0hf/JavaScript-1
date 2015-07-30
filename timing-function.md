# 浅谈 javascript 语法和定时函数  
  
## JavaScript 基本语法。

（一）数据类型与变量类型。 整数，小数，布局,字符串，日期时间，数组 强制转换： parseInt() parseFloat() isNaN()

（二）数组 var 数组名 = new Array ([长度]); //“假冒”数组 a.length-长度 a[下标] = 值。 a[下标]

（三）函数
  
```
[js] view plaincopy
function 函数名(形参)  
  
{  
  
}  
  
function ShowStr(a)  
  
{  
  
}  
```  

## DOM 操作

DOM - Document Object Model 文档对象模型。 树

线状模型，树状模型，网状模型

window

history

location

document
<html></html>

head

body

a,img,table,ul,ol.....

status
对象――object 特点的名词 行为的动词

1. window 1.alert() window.alert();

2. [var a = ]window.confirm("你能跑过豹子吗？何问起"); //prompt(); --不常用，不用记，输入

3. open(); open("地址","_blank/_self","新窗口的特点"); [var a = ]window.open("http://jb51.net"); 在新窗口中打开页面，返回新的窗口。a也是一个 window 类型的变量。 详细需要翻译。

4. close(); window.close();

5. setTimeout("code",毫秒数) 指定的毫秒数后，执行code一次。

举例  
  
```
[js] view plaincopy
<script language="javascript">  
  
function showTime()  
{  
var date = new Date();  
document.getElementById("hh").innerHTML = date;  
window.setTimeout("showTime()",1000);  
  
}  
window.setTimeout("showTime()",1000);  
</script>  
```  

练习

做一个5秒后出现何问起首页的页面
  
```
[js] view plaincopy
<script language="javascript">  
  
var a;  
function openAD()  
{  
a = window.open("http://hovertree.com","_blank","width=200 height=200 toolbar=no top=0 left=0");  
  
window.setTimeout("closeAD()",5000);  
}  
function closeAD()  
{  
a.close();  
}  
window.setTimeout("openAD()",5000);  
  
</script>  
```

以上所述就是本文的全部内容了，希望大家能够喜欢。