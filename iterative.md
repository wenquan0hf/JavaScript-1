# 浅谈 javascript 迭代方法  
  
五个迭代方法 都接受两个参数：要在每一项上运行的函数 和 运行该函数的作用域（可选）
every():对数组中的每一项运行给定函数。如果函数对每一项都返回 true，则返回 true。

filter():对数组中的每一项运行给定函数。返回该函数会返回 true 的项组成的数组。

forEach():对数组中每一项运行给定函数。该函数没有返回值。

map():对数组中每一项运行给定函数。返回每次函数调用的结果组成的函数。

some():对数组中每一项运行给定函数。如果函数对 任一项返回 true，则返回 true  
  
```
[js] view plaincopy
var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];  
  
        //every()和some()最相似  
  
        //every()  item:当前遍历项，index:当前项索引，array:数组对象本身  
  
        var everyResult = numbers.every(function (item, index, array) {  
  
            return item > 2;  
  
        });  
  
        alert(everyResult);//false  
  
        //some()  
  
        var someResult = numbers.some(function (item, index, array) {  
  
            return item > 2;  
  
        });  
  
        alert(someResult);//true  
  
        //filter  
  
        var filterResult = numbers.filter(function (item, index, array) {  
  
            return item > 2;  
  
        });  
  
        alert(filterResult);//[3,4,5,4,3]  
  
        //map()  
  
        var mapResult = numbers.map(function (item, index, array) {  
  
            return (item * 2);  
  
        });  
  
        alert(mapResult);//[2,4,6,8,10,8,6,4,2]  
  
        //forEach 本质上和for循环没有区别  
  
        var forEachResult=numbers.forEach(function(item,index,array){  
  
            alert(item)  
  
        });  
```   

以上就是本文的全部内容了，希望能给大家一些提示，能够更好的理解 javascript 迭代方法。