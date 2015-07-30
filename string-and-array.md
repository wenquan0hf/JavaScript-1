# 浅谈 javascript 中字符串 String 与数组 Array    
  
简单点就是 string 是字符(串)...

而 array 是数组...可以放数字啊,字符啊等一系列东东!!!
上个示例：
  
```
[js] view plaincopy
var str = "liuzhanqi";  
  
document.write(str["length"]);//等价str.l ength    
  
var str = string.fromcharcode(72, 101, 108, 108, 111, 33);  
  
document.write(str); //各整数作为unicode编码，并连接成字符串。  
  
var str1 = "liu".localecompare("zhan");//按本系统提供的默认比较规则比较当前字符串与参数字符串  
  
document.write(str1);//  
  
var str2 = "liu".slice(1);//在当前字符串中提取一个子字符串  
  
document.writeln(str2);  
  
document.writeln("se nki".split(' '));//用分隔符分割字符串，返回一个数组。  
  
document.write('liu'.fontcolor('red'));  
  
document.write('liu'.fixed());//使字符串显示为等宽字  
  
document.write('liu'.strike());//在字符串上添加删除线  
  
document.write("senki".sub());//显示为下标  
  
document.write("senki".sup());//显示为上标  
```  

charCodeAt(index) 返回字符串 index 处的字符的 Unicode 编码，该值是在 0~65535 之间的整数，若 index 超出了范围，则返回 NaN。

concat(str2) 将字符串 str2 连接在当前字符串组成一个新的字符串。

anchor(anchar_name) 创建一个锚点

link(url) 加上超链接

charAt(index) 返回字符串中 index 指定位置处的一个字符

小伙伴们是否了解了 javascript 中字符串 String 与数组 Arry 了呢，有疑问请给我留言吧，大家共同进步。
