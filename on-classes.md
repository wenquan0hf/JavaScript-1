# 浅谈 JavaScript 实现面向对象中的类

对象，是人们要进行研究的任何事物，从最简单的整数到复杂的飞机等均可看作对象，它不仅能表示具体的事物，还能表示抽象的规则、计划或事件。-- 引自百度百科

面向对象编程，是当前最流行的编程模式。但令人沮丧的是，作为前端应用最为广泛的 javascript，并不支持面向对象。

javascript 没有访问控制符，它没有定义类的关键字 class，它没有支持继承的 extend 或冒号，它也没有用 来支持虚函数的 virtual，不过，Javascript 是一门灵活的语言，下面我们就看看没有关键字 class 的 Javascript 如何实现类定 义，并创建对象。

## 定义类并创建类的实例对象

在 Javascript 中，我们用 function 来定义类，如下：

```
function Shape()  
  
{  
  
    var x=1;  
  
    var y=2;  
  
}  
```

你或许会说，疑？这个不是定义函数吗？没错，这个是定义函数，我们定义了一个 Shape 函数，并对 x 和 y 进行了初始化。不过，如果你换个角度来 看，这个就是定义一个 Shape 类，里面有两个属性 x 和 y，初始值分别是 1 和 2，只不过，我们定义类的关键字是 function 而不是 class。

然后，我们可以创建 Shape 类的对象 aShape，如下：

```
var aShape = new Shape();  
```

## 定义公有属性和私有属性

我们已经创建了 aShape 对象，但是，当我们试着访问它的属性时，会出错，如下：

```
aShape.x=1;  
```

这说明，用 var 定义的属性是私有的。我们需要使用 this 关键字来定义公有的属性。

```
function Shape()  
  
{  
  
    this.x=1;  
  
    this.y=2;  
  
}  
```

这样，我们就可以访问 Shape 的属性了，如：

```
aShape.x=2;  
```

好，我们可以根据上面的代码总结得到：用 var 可以定义类的 private 属性，而用 this 能定义类的 public 属性。

## 定义公有方法和私有方法

在 Javascript 中，函数是 Function 类的实例，Function 间接继承自 Object，所以，函数也是一个对象，因此，我们可以用赋值的方法创建函数，当然，我们也可以将一个函数赋给类的一个属性变量，那么，这个属性变量就可以称为方法，因为它是一个可以执行的函数。代码如下：

```
function Shape()  
  
{  
  
    var x=0;  
  
    var y=1;  
  
    this.draw=function()  
  
    {  
  
        //print;  
  
    };  
  
}  
```

我们在上面的代码中定义了一个 draw，并把一个 function 赋给它，下面，我们就可以通过 aShape 调用这个函数，OOP 中称为公有方法，如：

```
aShape.draw();  
```

如果用 var 定义，那么这个 draw 就变成私有的了，OOP 中称为私有方法，如：

```
function Shape()  
  
{  
  
    var x=0;  
  
    var y=1;  
  
    var draw=function()  
  
    {  
  
        //print;  
  
    };  
  
}  
```

这样就不能使用 aShape.draw 调用这个函数了。

## 构造函数

Javascript 并不支持 OOP，当然也就没有构造函数了，不过，我们可以自己模拟一个构造函数，让对象被创建时自动调用，代码如下：

```
function Shape()  
  
{  
  
    var init = function()  
  
    {  
  
         // 构造函数代码  
  
    };  
  
    init();  
  
}  
```

在 Shape 的最后，我们人为的调用了 init 函数，那么，在创建了一个 Shape 对象是，init 总会被自动调用，可以模拟我们的构造函数了。

## 带参数的构造函数

如何让构造函数带参数呢？其实很简单，将要传入的参数写入函数的参数列表中即可，如：

```
function Shape(ax,ay)  
  
{  
  
    var x=0;  
  
    var y=0;  
  
    var init = function()  
  
    {  
  
        // 构造函数  
  
        x=ax;  
  
        y=ay;  
  
    };  
  
    init();  
  
}  
```

这样，我们就可以这样创建对象：

```
var aShape = new Shape(0,1);  
```

## 静态属性和静态方法

在 Javascript 中如何定义静态的属性和方法呢？如下所示：

```
function Shape(ax,ay)  
  
{  
  
    var x=0;  
  
    var y=0;  
  
    var init = function()  
  
    {  
  
        // 构造函数  
  
        x=ax;  
  
        y=ay;  
  
    };  
  
    init();  
  
}  
  
Shape.count=0;// 定义一个静态属性 count，这个属性是属于类的，不是属于对象的。  
  
Shape.staticMethod=function(){};// 定义一个静态的方法  
```

有了静态属性和方法，我们就可以用类名来访问它了，如下：

```
alert (aShape.count);  
  
aShape.staticMethod();  
```

注意：静态属性和方法都是公有的，目前为止，我不知道如何让静态属性和方法变成私有的～

## 在方法中访问本类的公有属性和私有属性

在类的方法中访问自己的属性，Javascript 对于公有属性和私有属性的访问方法有所不同，请大家看下面的代码：

```
function Shape(ax,ay)  
  
{  
  
    var x=0;  
  
    var y=0;  
  
    this.gx=0;  
  
    this.gy=0;  
  
    var init = function()  
  
    {  
  
        x=ax;// 访问私有属性，直接写变量名即可  
  
        y=ay;  
  
        this.gx=ax;// 访问公有属性，需要在变量名前加上 this.  
  
        this.gy=ay;  
  
    };  
  
    init();  
  
}  
```

## this 的注意事项

根据笔者的经验，类中的 this 并不是一直指向我们的这个对象本身的，主要原因还是因为 Javascript 并不是 OOP 语言，而且，函数和类均用 function 定义，当然会引起一些小问题。

this 指针指错的场合一般在事件处理上面，我们想让某个对象的成员函数来响应某个事件，当事件被触发以后，系统会调用我们这个成员函数，但是，传入的 this 指针已经不是我们本身的对象了，当然，这时再在成员函数中调用 this 当然会出错了。

解决方法是我们在定义类的一开始就将 this 保存到一个私有的属性中，以后，我们可以用这个属性代替 this。我用这个方法使用 this 指针相当安全，而且很是省心～

我们修改一下代码，解决 this 问题。对照第六部分的代码看，你一定就明白了：

```
function Shape(ax,ay)  
  
{  
  
    var _this=this; // 把 this 保存下来，以后用_this 代替 this，这样就不会被 this 弄晕了  
  
    var x=0;  
  
    var y=0;  
  
    _this.gx=0;  
  
    _this.gy=0;  
  
    var init = function()  
  
    {  
  
        x=ax;// 访问私有属性，直接写变量名即可  
  
        y=ay;  
  
        _this.gx=ax;// 访问公有属性，需要在变量名前加上 this.  
  
        _this.gy=ay;  
  
    };  
  
    init();  
  
}  
```

以上我们聊了如何在 Javascript 中定义类，创建类的对象，创建公有和私有的属性和方法，创建静态属性和方法，模拟构造函数，并且讨论了容易出错的 this。

关于 Javascript 中的 OOP 实现就聊到这里，以上是最实用的内容，一般用 Javascript 定义类，创建对象用以上的代码已经足够 了。当然，你还可以用 mootools 或 prototype 来定义类，创建对象。我用过 mootools 框架，感觉很不错，它对 Javascript 的类 模拟就更完善了，还支持类的继承，有兴趣的读者可以去尝试一下。当然，如果使用了框架，那么在你的网页中就需要包含相关的 js 头文件，因此我还是希望读者 能够在没有框架的情况下创建类，这样，代码效率较高，而且你也可以看到，要创建一个简单的类并不麻烦～