# 浅谈 Javascript 中深复制

在 javascript 中，所有的 object 变量之间的赋值都是传地址的，可能有同学会问哪些是 object 对象。举例子来说明可能会比较好：

```
typeof(true)    //"boolean"  
  
typeof(1)       //"number"  
  
typeof("1")     //"string"  
  
typeof({})      //"object"  
  
typeof([])      //"object"  
  
typeof(null)    //"object"  
  
typeof(function(){})  //"function"  
```

所以其实我们深复制主要需要处理的对象就是 object 对象，非 object 对象只要直接正常的赋值就好。我实现 js 深复制的思路就是：

遍历所有该对象的属性，

如果该属性是 "object" 则需要特殊处理，

如果这个 object 对象比较特殊，是一个数组，那就创建一个新的数组并深复制数组里的元素

如果这个 object 对象是个非数组对象，那直接再对它递归调用深复制方法即可。

如果不是 "object"，则直接正常复制就行。

下面就是我的实现了：

```
Object.prototype.DeepCopy = function () {  
  
  var obj, i;  
  
  obj = {};  
  for (attr in this) {  
  
    if (this.hasOwnProperty(attr)) {  
  
      if (typeof(this[attr]) === "object") {  
  
        if (this[attr] === null) {  
  
          obj[attr] = null;  
  
        }  
  
        else if (Object.prototype.toString.call(this[attr]) === '[object Array]') {  
  
          obj[attr] = [];  
  
          for (i=0; i<this[attr].length; i++) {  
  
            obj[attr].push(this[attr][i].DeepCopy());  
  
          }  
  
        } else {  
  
          obj[attr] = this[attr].DeepCopy();  
  
        }  
  
      } else {  
  
        obj[attr] = this[attr];  
  
      }  
  
    }  
  
  }  
  
  return obj;  
  
};  
```

如果浏览器支持 ECMAScript 5 的话，为了深复制对象属性的所有特性，可以使用

```
Object.defineProperty(obj, attr, Object.getOwnPropertyDescriptor(this, attr));  
```

来替代

```
obj[attr] = this[attr];  
```

直接在 Object.prototype 上实现该方法的好处是，所有对象都会继承该方法。坏处是某些库也会改写 Object 对象，所以有时会发生冲突。这是需要注意的。具体使用方法如下：

```
Object.prototype.DeepCopy = function () { ...}  
  
var a = {x:1};  
  
var b = a;  
  
var c = a.DeepCopy();  
  
a.x = 2;  
  
b.x = 3;  
  
console.log(a.x);   //3  
  
console.log(b.x);   //3  
  
console.log(c.x);   //1  
```

以上就是关于深复制的讲解了，不过今天既然我们讲了深复制，那么想对应的还有浅复制，我们就来简单总结下他们之间的异同吧。

浅复制 (影子克隆): 只复制对象的基本类型, 对象类型, 仍属于原来的引用。

深复制 (深度克隆): 不紧复制对象的基本类, 同时也复制原对象中的对象。就是说完全是新对象产生的。