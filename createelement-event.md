# 浅谈 javascript 中 createElement 事件


createElement 是 HTML 中应用 W3C DOM 对像模型建立子节点也就是子元素的概念

```
<script>  
  
   window.onload = function () {  
  
   var input  = document.createElement('input');  
  
   var button = document.createElement('input');  
  
   input.type ='text';    
  
   input.id= 'text';  
  
   input.value ='1';  
  
   button.type='button';  
  
   button.value ='逐加';  
  
   button.style.width = '40px';  
  
   button.style.height = '23px';  
  
   document.body.appendChild(input);  
  
   document.body.appendChild(button);  
  
   button.onclick = function(){  
  
      var value = input.value;  
  
      input.value = value * 1 + 1;  
  
   }  
  
  }  
  
 </script>  
```

注：value 其实是一个字符，如果将 input.value=value\*1+1；换成 input.value=value+1；则结果会出现 111111，他是不断以字符形式加 1 的，所以这时候 value\*1 的就能将 value 值转换成 Int 型了。

## 总结：

要最终解决 createElement 方法的兼容性问题，还是要注意判断浏览器，针对 IE 可以使用其特有的通过为 createElement 传入一段合法的 HTML 代码字符串作为参数的方法，非 IE 浏览器仍然使用 W3C 规范的标准方法。
