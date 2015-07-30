# 浅谈 javascript 归并方法  
  
ECMAScript5 还新增了2个归并数组的方法：reduce()和reduceRight()。

这两个都会迭代数组的所有项

reduce():从第一项开始逐个遍历到最后。

reduceRight():从数组的最后一项开始，遍历到数组的第一项。
这两个方法都接受两个参数：在每一项上调用的函数（参数为：前一个值，当前值，项的索引，数组对象）

这个函数返回的任何值斗殴会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，

因此第一个参数是数组的第一项，第二个参数是数组的第二项

和 作为归并基础的初始值。

使用 reduce() 方法可以执行数组中所有值之和的操作，比如：  
  
```
[js] view plaincopy
var values = [1, 2, 3, 4, 5];  
  
        var sum = values.reduce(function (prev, cur, index, array) {  
  
            return prev + cur;  
  
        });  
  
        alert(sum);  
  
        //结果一样，只是方向相反而已  
  
        var sum2=values.reduceRight(function (prev,cur,index,array) {  
  
            return prev+cur;  
  
        });  
  
        alert(sum2);  
```  

归并排序（Merge sort）是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。

归并（Merge）排序法是将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。然后再把有序子序列合并为整体有序序列。

归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。