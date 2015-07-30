# 浅谈 javascript 的原型继承

请看源码： 

```
function clone(o) {   
var F = function(){};   
F.prototype = o;   
return new F();   
}  
```

首先看 ext（4.1 的 1896 行开始）的原型式继承。 

```
var TemplateClass = function(){};   
var ExtObject = Ext.Object = {   
chain: function (object) {   
TemplateClass.prototype = object;   
var result = new TemplateClass();   
TemplateClass.prototype = null;   
return result;   
}   
}  
```

这里清除了 object 的 prototype。 

再看一下 jquery 是怎么玩的继承。 

```
var jQuery = function(selector, context) {   
return new jQuery.fn.init(selector, context, rootjQuery);   
};   
-----------------------   
jQuery.fn = jQuery.prototype = {   
constructor: jQuery,   
init: function(selector, context, rootjQuery) {   
-----------------------   
}   
}   
-------------------   
jQuery.fn.init.prototype = jQuery.fn;  
```

jquery 玩的就比较高，借助 jQuery.fn.init 来完成，但是思路一样。
 
司徒正美的 mass 里也有类似的继承，在 lang_fix.js 里面第 17 行： 

```
create: function(o){   
if (arguments.length> 1) {   
$.log("Object.create implementation only accepts the first parameter.")   
}   
function F() {}   
F.prototype = o;   
return new F();   
}  
```

查看了一下 es5 的官方，找到了他的兼容补丁： 

```
// ES5 15.2.3.5   
// <a href="http://es5.github.com/#x15.2.3.5"target="_blank">http://es5.github.com/#x15.2.3.5</a>   
if (!Object.create) {   
Object.create = function create(prototype, properties) {   
var object;   
if (prototype === null) {   
object = { "__proto__": null };   
} else {   
if (typeof prototype != "object") {   
throw new TypeError("typeof prototype["+(typeof prototype)+"] !='object'");   
}   
var Type = function () {};   
Type.prototype = prototype;   
object = new Type();   
// IE has no built-in implementation of `Object.getPrototypeOf`   
// neither `__proto__`, but this manually setting `__proto__` will   
// guarantee that `Object.getPrototypeOf` will work as expected with   
// objects created using `Object.create`   
object.__proto__ = prototype;   
}   
if (properties !== void 0) {   
Object.defineProperties(object, properties);   
}   
return object;   
};   
}  
```

上面的代码考虑的就比较全面，但是需要另外引入 Object.defineProperties 的补丁才行，源码相对就比较多了。 

```
// ES5 15.2.3.6   
// <a href="http://es5.github.com/#x15.2.3.6"target="_blank">http://es5.github.com/#x15.2.3.6</a>   
// Patch for WebKit and IE8 standard mode   
// Designed by hax <hax.github.com>   
// related issue: <a href="https://github.com/kriskowal/es5-shim/issues#issue/5"target="_blank">https://github.com/kriskowal/es5-shim/issues#issue/5</a>   
// IE8 Reference:   
// <a href="http://msdn.microsoft.com/en-us/library/dd282900.aspx"target="_blank">http://msdn.microsoft.com/en-us/library/dd282900.aspx</a>   
// <a href="http://msdn.microsoft.com/en-us/library/dd229916.aspx"target="_blank">http://msdn.microsoft.com/en-us/library/dd229916.aspx</a>   
// WebKit Bugs:   
// <a href="https://bugs.webkit.org/show_bug.cgi?id=36423"target="_blank">https://bugs.webkit.org/show_bug.cgi?id=36423</a>   
function doesDefinePropertyWork(object) {   
try {   
Object.defineProperty(object, "sentinel", {});   
return "sentinel" in object;   
} catch (exception) {   
// returns falsy   
}   
}   
// check whether defineProperty works if it's given. Otherwise,   
// shim partially.   
if (Object.defineProperty) {   
var definePropertyWorksOnObject = doesDefinePropertyWork({});   
var definePropertyWorksOnDom = typeof document == "undefined" ||   
doesDefinePropertyWork(document.createElement("div"));   
if (!definePropertyWorksOnObject || !definePropertyWorksOnDom) {   
var definePropertyFallback = Object.defineProperty;   
}   
}   
if (!Object.defineProperty || definePropertyFallback) {   
var ERR_NON_OBJECT_DESCRIPTOR = "Property description must be an object:";   
var ERR_NON_OBJECT_TARGET = "Object.defineProperty called on non-object:"   
var ERR_ACCESSORS_NOT_SUPPORTED = "getters & setters can not be defined" +   
"on this javascript engine";   
Object.defineProperty = function defineProperty(object, property, descriptor) {   
if ((typeof object != "object" && typeof object != "function") || object === null) {   
throw new TypeError(ERR_NON_OBJECT_TARGET + object);   
}   
if ((typeof descriptor != "object" && typeof descriptor != "function") || descriptor === null) {   
throw new TypeError(ERR_NON_OBJECT_DESCRIPTOR + descriptor);   
}   
// make a valiant attempt to use the real defineProperty   
// for I8's DOM elements.   
if (definePropertyFallback) {   
try {   
return definePropertyFallback.call(Object, object, property, descriptor);   
} catch (exception) {   
// try the shim if the real one doesn't work   
}   
}   
// If it's a data property.   
if (owns(descriptor, "value")) {   
// fail silently if "writable", "enumerable", or "configurable"   
// are requested but not supported   
/*  
// alternate approach:  
if (// can't implement these features; allow false but not true  
!(owns(descriptor,"writable") ? descriptor.writable : true) ||  
!(owns(descriptor,"enumerable") ? descriptor.enumerable : true) ||  
!(owns(descriptor,"configurable") ? descriptor.configurable : true)  
)  
throw new RangeError(  
"This implementation of Object.defineProperty does not" +  
"support configurable, enumerable, or writable."  
);  
*/   
if (supportsAccessors && (lookupGetter(object, property) ||   
lookupSetter(object, property)))   
{   
// As accessors are supported only on engines implementing   
// `__proto__` we can safely override `__proto__` while defining   
// a property to make sure that we don't hit an inherited   
// accessor.   
var prototype = object.__proto__;   
object.__proto__ = prototypeOfObject;   
// Deleting a property anyway since getter / setter may be   
// defined on object itself.   
delete object[property];   
object[property] = descriptor.value;   
// Setting original `__proto__` back now.   
object.__proto__ = prototype;   
} else {   
object[property] = descriptor.value;   
}   
} else {   
if (!supportsAccessors) {   
throw new TypeError(ERR_ACCESSORS_NOT_SUPPORTED);   
}   
// If we got that far then getters and setters can be defined !!   
if (owns(descriptor, "get")) {   
defineGetter(object, property, descriptor.get);   
}   
if (owns(descriptor, "set")) {   
defineSetter(object, property, descriptor.set);   
}   
}   
return object;   
};   
}   
// ES5 15.2.3.7   
// <a href="http://es5.github.com/#x15.2.3.7"target="_blank">http://es5.github.com/#x15.2.3.7</a>   
if (!Object.defineProperties) {   
Object.defineProperties = function defineProperties(object, properties) {   
for (var property in properties) {   
if (owns(properties, property) && property != "__proto__") {   
Object.defineProperty(object, property, properties[property]);   
}   
}   
return object;   
};   
}  

EcmaScript6 的类继承。 
[js] view plaincopy
class module extends Base {   
constructor() {   
}   
}  
```

越玩越像 java 了，不过 es6 很多浏览器还不支持。
 
最后推荐的写法： 

```
if (!Object.create) {   
Object.create = function create(o) {   
var F = function(){};   
F.prototype = o;   
var result = new F();   
F.prototype = null;   
return result;   
}   
}  
```
