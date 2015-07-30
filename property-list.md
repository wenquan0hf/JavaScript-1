# 浅谈 JavaScript 事件的属性列表  
  
HTML 4.0 的新特性之一是能够使 HTML 事件触发浏览器中的行为，比如当用户点击某个 HTML 元素时启动一段 JavaScript。下面是一个属性列表，可将之插入 HTML 标签以定义事件的行为。





属性
此事件发生在何时...


onabort
图像的加载被中断。


onblur
元素失去焦点。


onchange
域的内容被改变。


onclick
当用户点击某个对象时调用的事件句柄。


ondblclick
当用户双击某个对象时调用的事件句柄。


onerror
在加载文档或图像时发生错误。


onfocus
元素获得焦点。


onkeydown
某个键盘按键被按下。


onkeypress
某个键盘按键被按下并松开。


onkeyup
某个键盘按键被松开。


onload
一张页面或一幅图像完成加载。


onmousedown
鼠标按钮被按下。


onmousemove
鼠标被移动。


onmouseout
鼠标从某元素移开。


onmouseover
鼠标移到某元素之上。


onmouseup
鼠标按键被松开。


onreset
重置按钮被点击。


onresize
窗口或框架被重新调整大小。


onselect
文本被选中。


onsubmit
确认按钮被点击。


onunload
用户退出页面。


鼠标 / 键盘属性


属性
描述


altKey
返回当事件被触发时，"ALT" 是否被按下。


button
返回当事件被触发时，哪个鼠标按钮被点击。


clientX
返回当事件被触发时，鼠标指针的水平坐标。


clientY
返回当事件被触发时，鼠标指针的垂直坐标。


ctrlKey
返回当事件被触发时，"CTRL" 键是否被按下。


metaKey
返回当事件被触发时，"meta" 键是否被按下。


relatedTarget
返回与事件的目标节点相关的节点。


screenX
返回当某个事件被触发时，鼠标指针的水平坐标。


screenY
返回当某个事件被触发时，鼠标指针的垂直坐标。


shiftKey
返回当事件被触发时，"SHIFT" 键是否被按下。


常用HTML元素的事件：

onclick（单击）、

ondblclick（双击）、

onkeydown（按键按下）、

onkeypress（点击按键）、

onkeyup（按键释放）、

onmousedown（鼠标按下）、

onmousemove（鼠标移动）、

onmouseout（鼠标离开元素范围）、

onmouseover（鼠标移动到元素范围）、

onmouseup（鼠标按键释放）、

oncontextmenu（在浏览器中单击鼠标右键显示”右键菜单”时触发）
以上就是本文的全部内容了，希望大家能够喜欢。