# 浅谈 javascript 的调试  
  
最近比较吐槽，大家都知道，现在 web 前端相对几年前来说已经变得很重了，各种 js 框架，各种面对对象，而且项目多了，就会提取公共模块。

　　这些模块的 UI 展示都一样，不一样的就是后台逻辑，举个例子吧，我们做企业差旅的时候，通常都有一个成本中心的js公共模块，客户在预定机票的时候来填写这个成本中心，而这种成本中心分布在 online，offline 和 app 等预定端，这样也是方便后期和客户公司进行月结算。

　　我们还知道，项目做大了，复杂化了，SOA 化了之后，很多问题就来了，就像 web 中的一个理论，所有前端的数据都是不可信的，那对方团队的接口数据又何尝不是，以前项目小的时候，不会那么不自信，也只会在 Logic error 的时候会记录下日志，正常的业务流程一般很少记录，毕竟 info 日志看着不美观，而且还会消耗服务器带宽，也还会拖累 web 的性能。

　　但是项目大了，当你某天在项目中遇到了奇怪的 bug 时，你靠着残缺不全的日志，好不容易用肉眼逐行追溯到了接口，但是参数太多，无法准确的还原接口的参数数据，但是你又100%的自信认定肯定就是接口的返回问题，但是又拿不出完整的报文，这时候你又没法找接口提供方，当时那个无奈呀，多想最好每行都有日志该多好啊。

　　有了教训后，记流程日志的趋势越来越盛行，最终也酿造了一个年初的大事件，稀里糊涂的说了一大堆，web 后端如此，那现在的重前端不也一样要记录日志么？我们知道既然是公共的js模块，那这个模块肯定自己封装了一些方法，肯定是绝对不可以让第三方程序去操作它自己的文本结点，比如下面这样：
  
```
[js] view plaincopy
<!--third  module -->  
  
公司：<input type="text" id="company" value="xxx有限公司" />  
  
员工姓名：<input type="text" id="username" value="张三" />  
  
<!-- -->  
  
<script type="text/javascript">  
  
    //成本中心  
  
    var costCenter = (function () {  
  
         var company = (document.getElementById("company") || "") && document.getElementById("company").value;  
  
         var username = (document.getElementById("username") || "") && document.getElementById("username").value;  
  
         var result = {  
  
             getInfo: function () {  
  
                 return { company: company, username: username };  
  
             },  
  
             validation: function () {  
  
                 return Boolean(company && username);  
  
             }  
  
         };  
  
         return result;  
  
     })();  
  
 </script>  
```  

　　为了简化操作，第三方UI提供了公司名和员工姓名的 UI 结点，并且封装了一个 costcenter 类来提供读取方法，可以看到，我的预定程序只需读取 costCenter.getInfo就好了，也起到了一个封装的作用。

　　但是问题就出现在这里，项目实战中会因为各种原因导致在 costcenter 中取不到值，当然也可能是 common ui的bug。

　　但是当时你又不能非常确定是否真的取到了值，但是在逻辑上就算取不到值，原则上你也不能阻止订单提交，所以为了彻底追踪 bug，就写了个 logCenter 单例类来记录日志。通常用js 来记录 log 有这种方法。

<1> ajax

　　这种方式很容易想到，但是你使用原生的 xmlhttprequest 的话，还需要考虑浏览器兼容，但不用原生的话，就要借助于第三方框架，比如 jquery，但是毕竟还是有很多公司是不使用jquery 的，所以这个要根据实际的需要来使用了。
  
```
[js] view plaincopy
//日志中心  
  
    var logCenter = (function () {  
  
        var result = {  
  
            info: function (title, message) {  
  
                //ajax操作  
  
                $.get("<a href="http://xxx.com" target="_blank">http://xxx.com</a>", { "title": title, "message": message }, function () {  
  
                }, "post");  
  
             }  
  
         };  
  
         return result;  
  
     })();  
```  

<2>image

　　我们的 dom 中有一个叫做 image 的对象，所以可以通过动态给它的 src 赋值来达到请求后台 url 的目的，同时在 url 中加上我们需要传递 title 和 message 信息，这种动态给 image.src 的方式是不需要考虑浏览器兼容性的问题，非常不错。
  
```
[js] view plaincopy
//日志中心  
  
    var logCenter = (function () {  
  
        var result = {  
  
            info: function (title, message) {  
  
                //ajax操作  
  
                $.get("<a href="http://xxx.com" target="_blank">http://xxx.com</a>", { "title": title, "message": message }, function () {  
  
                }, "post");  
  
             },  
  
             info_image: function (title, message) {  
  
                 //image  
  
                 var img = new Image();  
  
                 img.src = "<a href="http://www.baidu.com?title" target="_blank">http://www.baidu.com?title</a>=" + title + "&message=" + message + "&temp=" + (Math.random() * 100000);  
  
             }  
  
         };  
  
         return result;  
  
     })();  
```  

以上就是本文的主要内容了，后续我们将继续深入探讨