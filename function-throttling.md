# 浅谈 JavaScript 函数节流

浏览器中某些计算和处理要比其他的昂贵的多。例如，DOM 操作比起非 DOM 交互需要更多的内存和 CPU 时间。连续尝试进行过多的 DOM 相关操作可能会导致 浏览器挂起，有时候甚至会崩溃。尤其在 IE 中使用 onresize 事件处理程序的时候容易发生，当调整浏览器大小的时候，该事件连续触发。在 onresize 事件处理程序内部如果尝试进行 DOM 操作，其高频率的更改可能会让浏览器崩溃。

函数节流背后的基本思想是，某些代码不可以在没有间断的情况连续重复执行。第一次调用函数，创建一个定时器，在指定的时间间隔之后运行代码。当第二次调用 该函数时，它会清除前一次的定时器并设置另一个。如果前一个定时器已经执行过了，这个操作就没有任何意义。然而，如果前一个定时器尚未执行，其实就是将其 替换为一个新的定时器。目的是只有在执行函数的请求停止了一段时间之后才执行。

```
function throttle (method , context){  
  
                clearTimeout (method.tId);  
  
                method.tId = setTimeout ( function () {  
  
                     method.call (context);  
  
                 } , 100);  
  
          }  
```

### 应用举例:

假设有一个 <div/\> 元素需要保持它的高度始终等同于宽度，可作如下编码:

```
function resizeDiv(){  
  
                var div = document.getElementById("mydiv");  
  
                div.style.height = div.offsetWidth + "px";  
  
            }  
  
           window.onresize = function(){  
  
                throttle(resizeDiv);  
  
            }  
```

这里，调整大小的功能被放入了一个叫做 resizeDiv 的单独函数中，然后 onresize 事件处理程序调用 throttle()并传入 resizeDiv 函数，而不是直接调用 resizeDiv()。多数情况下，用户是感觉不到变化的，虽然给浏览器节省的计算可能非常大。