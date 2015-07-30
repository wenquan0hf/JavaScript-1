# 浅谈 Javascript 变量作用域问题

## Js 中的变量作用域问题：

### 没有块级作用域。Js 中的变量作用域不是以 {} 为界的，不像 C/C++/Java。

如：

```
if(true){  
  
     var name = "qqyumidi";  
  
 }  
  
               
  
 alert(name);  // 结果：qqyumidi  
```

Js 会将在 if 中定义的变量添加到当前的执行环境中，尤其在使用 for 循环时需要注意与其他语言的差异。

```
for(var i=0; i<10; i++){  
  
     ;  
  
 }  
  
   
  
 alert(i);   // 结果：10  
```

这里仅仅是个人的理解，如有纰漏，还请大家告之。