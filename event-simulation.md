# 浅谈 Javascript 事件模拟

这就意味着会有适当的事件冒泡，并且浏览器会执行分配的事件处理程序。这种能力在测试 web 应用程序的时候，是非常有用的，在 DOM 3 级规范中提供了方法来模拟特定的事件，IE9 chrome FF Opera 和 Safari 都支持这样的方式，在 IE8 及以前的办法的 IE 浏览器有他自己的方式来模拟事件 

###Dom 事件模拟 

可以通过 document 上的 createEvent()方法，在任何时候创建事件对象，此方法只接受一个参数，既要创建事件对象的事件字符串，在 DOM2 级规范上所有的字符串都是复数形式，在 DOM 3 级事件上所有的字符串都采用单数形式，所有的字符串如下： 

UIEvents：通用的 UI 事件，鼠标事件键盘事件都是继承自 UI 事件，在 DOM 3 级上使用的是 UIEvent。 

MouseEvents：通用的鼠标事件，在 DOM 3 级上使用的是 MouseEvent。 

MutationEvents：通用的突变事件，在 DOM 3 级上使用的是 MutationEvent。 

HTMLEvents：通用的 HTML 事件，在 DOM3 级上还没有等效的。 

注意，ie9 是唯一支持 DOM3 级键盘事件的浏览器，但其他浏览器也提供了其他可用的方法来模拟键盘事件。 

一旦创建了一个事件对象，就要初始化这个事件的相关信息，每一种类型的事件都有特定的方法来初始化，在创建完事件对象之后，通过 dispatchEvent()方法来将事件应用到特定的 dom 节点上，以便其支持该事件。这个 dispatchEvent()事件，支持一个参数，就是你创建的 event 对象。 

### 鼠标事件模拟 

鼠标事件可以通过创建一个鼠标事件对象来模拟（mouse event object），并且授予他一些相关信息，创建一个鼠标事件通过传给 createEvent()方法一个字符串 "MouseEvents"，来创建鼠标事件对象，之后通过 iniMouseEvent()方法来初始化返回的事件对象，iniMouseEvent()方法接受 15 参数，参数如下： 

type string 类型 ：要触发的事件类型，例如‘click'。 

bubbles Boolean 类型：表示事件是否应该冒泡，针对鼠标事件模拟，该值应该被设置为 true。 

cancelable bool 类型：表示该事件是否能够被取消，针对鼠标事件模拟，该值应该被设置为 true。 

view 抽象视图：事件授予的视图，这个值几乎全是 document.defaultView. 

detail int 类型：附加的事件信息这个初始化时一般应该默认为 0。 

screenX int 类型 ： 事件距离屏幕左边的 X 坐标 

screenY int 类型 ： 事件距离屏幕上边的 y 坐标 

clientX int 类型 ： 事件距离可视区域左边的 X 坐标 

clientY int 类型 ： 事件距离可视区域上边的 y 坐标 

ctrlKey Boolean 类型 ： 代表 ctrol 键是否被按下，默认为 false。 

altKey Boolean 类型 ： 代表 alt 键是否被按下，默认为 false。 

shiftKey Boolean 类型 ： 代表 shif 键是否被按下，默认为 false。 

metaKey Boolean 类型： 代表 meta key 是否被按下，默认是 false。 

button int 类型： 表示被按下的鼠标键，默认是零. 

relatedTarget (object)： 事件的关联对象. 只有在模拟 mouseover 和 mouseout 时用到。 

值得注意的是，initMouseEvent()的参数直接与 event 对象相映射，其中前四个参数是由浏览器用到，只有事件处理函数用到其他的参数，当事件对象作为参数传给 dispatch()方式，target 属性将会自动被赋上值。下面是一个例子， 

```
var btn = document.getElementById("myBtn");   
var event = document.createEvent("MouseEvents");   
event.initMouseEvent("click", true, true, document.defaultView, 0, 0, 0, 0, 0,false, false, false, false, 0, null);   
btn.dispatchEvent(event);  
```

在 DOM 实现的浏览器中，所有其他的事件都包括 dbclick，都可以通过相同的方式来实现。 

### 键盘事件模拟 

值得注意的是键盘事件已经从 DOM2 级事件中移出了，起初在 DOM2 级事件的草案版中，键盘事件是作为草案的一部分的，但在最终版被移出了，FF 已经实现了草案版中的键盘事件，值得注意的是在 DOM3 级事件中实现的键盘事件与 DOM2 级事件草案版中的键盘事件还是存在很大差异的。 

在 dom3 级事件中创建一个键盘事件对象是通过 createEvent()方法，并传入 KeyBoardEvent 字符串作为参数，对返回的 event 对象，调用 initKeyBoadEvent()方法初始化，初始化键盘事件的参数有以下几个： 

type (string) - 要触发的事件类型，例如 "keydown"。 

bubbles (Boolean) ― 代表事件是否应该冒泡。

cancelable (Boolean) ― 代表事件是否可以被取消。

view (AbstractView) ― 被授予事件的是图。通常值为：document.defaultView。

key (string) ― 按下的键对应的 code。 

location (integer) ― 按下键所在的位置。0：默认键盘，1 左侧位置，2 右侧位置，3 数字键盘区，4 虚拟键盘区，or 5 游戏手柄。 

modifiers (string) ― 一个有空格分开的修饰符列表。

repeat (integer) ― 一行中某个键被按下的次数。

请注意的是，在 DOM3 事件中，费掉了 keypress 事件，因此按照下面的方式，你只能模拟键盘上的 keydown 和 keyup 事件。 

```
var textbox = document.getElementById("myTextbox"),event;   
　　if (document.implementation.hasFeature("KeyboardEvents", "3.0")){   
　　　　event = document.createEvent("KeyboardEvent");   
　　　　event.initKeyboardEvent("keydown", true, true, document.defaultView, "a",0, "Shift", 0);   
　　}   
　　textbox.dispatchEvent(event);  
```

在 FF 下，允许你通过使用 document.createEvent('KeyEvents')，这种方式来创建键盘事件，初始化的方法为 initKeyEvent()，这个方法接受 10 个参数， 

type (string) ― 要触发的事件类型，例如 "keydown"。

bubbles (Boolean) ― 代表事件是否应该冒泡。

cancelable (Boolean) ― 代表事件是否可以被取消。

view (AbstractView) ― 被授予事件的是图. 通常值为：document.defaultView。 

ctrlKey (Boolean) ― 代表 ctrol 键是否按下。默认 false。

altKey (Boolean) ― 代表 alt 键是否按下。默认 false。

shiftKey (Boolean) ― 代表 shift 键是否按下。默认 false。

metaKey (Boolean) ― 代表 meta 键是否按下。默认 false。 

keyCode (integer) ― 键按下或释放时键所对应的键码。默认是 0； 

charCode (integer) ― 按下的键的字符所对应的 ASCII code。是共 keypress 事件使用的 默认是 0。 

### 模拟其他事件 

鼠标事件和键盘事件是在浏览器中最长被模拟的事件，但是某些时候同样需要模拟突变事件和 HTML 事件。可以用 createEvent('MutationEvents')，来创建一个突变事件对象，可以采用 initMutationEvent() 来初始化这个事件对象，参数包括 type，bubbles，cancelable， relatedNode，prevValue，newValue，attrName，和 attrChange。可以采用下面的方式来模拟一个突变事件： 

```
　　var event = document.createEvent('MutationEvents'); 
　　event.initMutationEvent("DOMNodeInserted", true, false, someNode, "","","",0); 
　　target.dispatchEvent(event); 
```

对于 HTML 事件，直接上代码。 

```
　　var event = document.createEvent("HTMLEvents"); 
　　event.initEvent("focus", true, false); 
　　target.dispatchEvent(event); 
```

对于突变事件和 HTML 事件是很少在浏览器中用到，因为他们收应用程序的限制。 

### 定制 DOM 事件 

在 DOM3 级事件中定义了一类事件称之为 custom event，我称之为客户事件，客户事件不会原生的被 dom 触发，而是直接提供，以至于开发者可以创建他们自己的事件，你可以创建一个自己的客户事件，通过调用 createEvent('CustomEvent')，对返回的事件对象调用，initCustomEvent()方法，其中传递四个参数 type，bubbles，cancelable，detail。ps:小弟对这部分理解有限，在这里只是抛砖引玉。 

### IE 中的事件模拟 

从 IE8，以及更早版本的 IE，都在模仿 DOM 模拟事件的方式：创建事件对象，初始化事件信息，之后触发事件。当然 IE 在完成这几个步骤的过程是不同的。 

首先不同于 dom 中创建 event 对象的方法，IE 采用 document.createEventObject()方法，并且没有参数，返回一个通用的事件对象，接下来要对返回的 event 对象赋值，此时 ie 并没有提供初始化函数，你只能采用物理方法一个一个的赋值，最后在目标元素上调用 fireEvent()方法，参数为两个：事件处理的名称和创建的事件对象。当 fireEvent 方法被调用的时候，event 对象的 srcElement 和 type 属性将会被自动赋值，其他将需要手动赋值。请看下面的例子： 

```
var btn = document.getElementById("myBtn");   
var event = document.createEventObject();   
event.screenX = 100;   
event.screenY = 0;   
event.clientX = 0;   
event.clientY = 0;   
event.ctrlKey = false;   
event.altKey = false;   
event.shiftKey = false;   
event.button = 0;   
btn.fireEvent("onclick", event);  
```

这个例子创建了一个事件对象，之后通过一些信息初始化该事件对象，注意事件属性的赋值是无序的，对于事件对象来说这些属性值不是很重要，因为只有事件句柄对应的处理函数（event handler）会用到他们。对于创建鼠标事件、键盘事件还是其他事件的事件对象之间是没有区别的，因为一个通用的事件对象，可以被任何类型的事件触发。 

值得注意的是，在 Dom 的键盘事件模拟中，对于一个 keypress 模拟事件的结果不会作为字符出现在 textbox 中，即使对应的事件处理函数已经触发。 

与 DOM 事件模拟相比，个人觉得 IE 的事件模拟更容易让人记忆和接受，统一的事件模型可以带来一些便捷。