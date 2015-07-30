# 浅谈 Javascript 鼠标和滚轮事件

## 鼠标事件　　 

鼠标事件也许是 web 页面当中最常用到的事件，因为鼠标是最常用的导航设备，在 DOM3 级事件上定义了 9 个鼠标事件，分别为： 

click：当用户点击鼠标主键通常是指鼠标左键或按回车键时触发。 

dbclick：当用户双击鼠标主键时发生触发，这个事件并没有在 DOM2 级事件中定义但是却被普遍支持了，后来在 DOM3 级中得到了标准化。 

mousedown：当用户按下鼠标任意一个键都会触发，这个事件是不能够通过键盘触发的。 

mouseenter：当鼠标图标从元素外移动至元素边界内时触发。该事件不支持冒泡，而且当鼠标在元素的上表面移动时负触发此事件。该事件不属于 DOM2 级事件，属于 DOM3 级后添加的事件，IE 、FF9+、以及 opera 支持此事件。 

mouseleave：当鼠标处于元素上方，之后移出元素边界是触发该事件，与 mouseenter 事件相同，不支持冒泡，在元素上方是不触发，该事件不属于 DOM2 级事件，属于 DOM3 级后添加的事件，IE 、FF9+、以及 opera 支持此事件。 

mousemove：当鼠标在某元素周围移动时重复触发，该事件不能通过键盘事件触发。 

mouseout：当鼠标处于某一元素上方，之后移动到其他元素上方时触发。元素移动到原始元素的边界外，或者原始元素的子元素上，这个事件不能通过键盘触发。 

mouseover：当用户将鼠标第一次从某元素边界外移动到该元素边界内时触发，这个事件不能够通过键盘触发。 

mouseup：当用户释放鼠标按键是触发，这个事件不能够通过键盘触发。 

页面上的所有元素都支持鼠标事件，除了 mouseenter 和 mouseleave 外，所有的事件都冒泡，并且他们的默认行为是可以被取消掉的。但取消鼠标事件的默认行为可能会影响到其他事件，因为有些鼠标事件是相互依赖的。 

只有当一个 mousedown 事件和一个 mouseup 事件在同一个元素上触发，才能触发鼠标的 click 事件；假设任何一个事件被取消，click 事件将永远不会被触发。相似的原理 dbclick 事件依赖于 click 事件，如果连续两次 click 事件中任意一次被取消，dbclick 都不会被触发。常用的鼠标事件如下: 

1.mousedown、2.mouseup、3.click、4.mousedown、5.mouseup、6.click、7.dbclick。 

无论是 click 还是 dbclick 事件，都依赖于其他事件的触发。 

你可以通过下面的代码来确定浏览器是否在 dom2 事件上支持鼠标事件， 

```
var isSupport = document.implementation.hasFeature('MouseEvents'，'2.0')； 
```

然而值得注意的是在 dom3 级事件上检测则有些不同： 

```
var isSupport = document.implementation.hasFeature('MouseEvent','3.0'); 
```

在鼠标事件上还包含一个子类事件，即 wheel 事件（滚轮事件）。在 wheel 事件中只包含一个事件，mousewheel 事件，他反应鼠标滚轮或其他装置，如 mac 的 touchpad 的交互情况。 

## 关联元素 

对于 mouseover 和 mouseout 事件来说，还存在着与事件相关的元素，这连个事件所执行的动作包括，移动鼠标从一个元素边界内部到另一个元素边界内部。以 mouseover 事件为例，他的主要目标元素是鼠标将要移至的元素，而那个关联元素就是失去鼠标的那个元素。同样对于 mouseout 事件，主要目标是那个鼠标移出的元素, 而关联元素则是获得鼠标的元素，DOM 通过 event 对象上的 relatedTarget 属性来提供关联元素的信息，IE8 或更早版本的 IE 不支持 relatedTarge 属性，但却提供了与其功能相类似的 fromElement 属性和 toElement 属性。在 IE 下，当 mousemove 事件触发时，event 对象的 fromElement 包含着关联元素，当 mouseout 事件触发时，event 的 toElement 属性包含着关联元素。在 IE9 中支持所有的属性，一个跨浏览器的 getRelatedTarget 方法可以这样写： 

```
var eventUtil = {   
getRelateTarget:function(event){   
if (event.relatedTarget) {   
return event.relatedTarget;   
}else if(event.fromElement){   
return event.fromElement;   
}else if(event.toElement){   
return event.toElement;   
}else {   
return null;   
}   
}   
};  
```

## buttons 

click 事件只有当鼠标主键点击了某一元素的时候才会触发（或者当某一元素获得焦点时按下回车键），对于 mousedown 和 mouseup 来说，在事件对象 event 上存在一个属性 button，他可以确定是哪个键按下或者释放。在 DOM 实现的 button 属性值通常有三种可能： 

0：代表主键， 

1：代表滚轮， 

2：代表副键。 

在一般情况下主键指的是鼠标的左键，副键代表鼠标右键。 

从 IE8 开始也提供了 button 属性，但却有着完全不同的取值范围：　 

0 ：没有按键按下， 

1 ：主键已经被按下， 

2 ：代表副键已经被按下， 

3 ：主键副键都被按下， 

4 ：代表中间键被按下， 

5 ：代表主键和中间件被按下， 

6 ：代表副键和中间键被按下， 

7 ：代表三个键都被按下。 

可见 DOM 模型下的 button 属性的取值范围比 IE 模型下的取值范围要简单的多，而且个人觉得 IE 下的操作情况略显变态。 

## 其他事件信息 

在 DOM2 级事件上，为事件对象 event 还提供了 detail 属性来提供更多的事件信息，例如对于点击事件来说，detail 可以记录同一像素位置的点击次数，detail 的值是从 1 开始计数的，每次点击后加一，如果在 mousedown 和 mouseup 之间，鼠标发生移动，这个值将会被清零。 