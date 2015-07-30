# 浅谈 JavaScript 中的 Math.atan() 方法的使用  
  
此方法返回弧度的反正切。atan 方法返回一个在 -pi/2 和 π/2 弧度之间的数值。

## 语法
  
```
[js] view plaincopy
Math.atan( x ) ;  
```

**下面是参数的详细信息：**

x : 一个数字

**返回值:**

返回一个数弧度的反正切值

**例子：**
  
```
[js] view plaincopy
<html>  
<head>  
<title>JavaScript Math atan() Method</title>  
</head>  
<body>  
<script type="text/javascript">  
  
var value = Math.atan(-1);  
document.write("First Test Value : " + value );   
   
var value = Math.atan(.5);  
document.write("<br />Second Test Value : " + value );   
  
var value = Math.atan(30);  
document.write("<br />Third Test Value : " + value );   
  
var value = Math.atan("string");  
document.write("<br />Fourth Test Value : " + value );   
</script>  
</body>  
</html>  
```

这将产生以下结果：

[js] view plaincopy
First Test Value : -0.7853981633974483  
Second Test Value : 0.4636476090008061  
Third Test Value : 1.5374753309166493  
Fourth Test Value : NaN  