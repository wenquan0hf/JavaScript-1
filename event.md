# 浅谈 JavaScript 的事件  
  
## 事件流  

事件流描述的是从页面中接收事件的顺序。但是 IE 提出的是冒泡流，而 Netscape Communicator 提出的是捕获流。

JavaScript 事件流  

## 事件冒泡（event bubbling）  

事件开始由最具体的元素（嵌套层次最深的那个节点）接收，然后逐级向上传播为较不为具体的节点（文档）。如下：
  
```
[js] view plaincopy
<html>  
  
    <head>  
  
        <title>事件冒泡</title>  
  
    </head>  
  
    <body>  
  
        <div id="myDiv">点击我</div>  
  
    </body>  
  
</html>  
  
window.onload = function(){  
  
    var obj = document.getElementById("test");  
  
    obj.onclick = function(){  
  
        alert(this.tagName);  
  
    };  
  
    document.body.onclick = function(){  
  
        alert(this.tagName);  
  
    };  
  
    document.documentElement.onclick = function(){  
  
        alert(this.tagName);  
  
    };  
  
    document.onclick = function(){  
  
        alert("document");  
  
    };  
  
    window.onclick = function(){  
  
        alert("window");  
  
    }  
  
};  
```  

事件传播顺序：div――>body――>html――>document

注意：

现代所有浏览器都支持冒泡事件，但实现还有一些差别。IE5.5 及更早版本中的事件冒泡会直接从 body 跳到 document（不执行 html ）。Firefox、Chrome 和 Safari 则将事件一直冒泡到 window 对象。  

## 停止事件冒泡和取消默认事件

### 获取事件对象
  
```
[js] view plaincopy
function getEvent(event) {  
  
// window.event IE  
  
// event 非IE  
  
return event || window.event;  
  
}  
```

### 功能:停止事件冒泡  
  
``` 
[js] view plaincopy
function stopBubble(e) {  
  
 // 如果提供了事件对象，则这是一个非IE浏览器  
  
 if ( e && e.stopPropagation ) {  
  
 // 因此它支持W3C的stopPropagation()方法  
  
 e.stopPropagation();  
  
} else {  
  
 // 否则，我们需要使用IE的方式来取消事件冒泡  
  
window.event.cancelBubble = true;  
  
}  
  
}  
```  

### 阻止浏览器的默认行为
  
```
[js] view plaincopy
function stopDefault( e ) {  
  
     // 阻止默认浏览器动作(W3C)  
  
     if ( e && e.preventDefault ) {  
  
         e.preventDefault();  
  
     } else {  
  
        // IE中阻止函数器默认动作的方式  
  
        window.event.returnValue = false;  
  
    }  
  
    return false;  
  
}  
```  