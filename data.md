# 浅谈 JavaScript Date 日期和时间对象  
  
## Date 日期和时间对象  

### 介绍

　　Date 对象，是操作日期和时间的对象。Date 对象对日期和时间的操作只能通过方法。

### 构造函数

#### new Date() ：返回当前的本地日期和时间

参数：无  

返回值：

{Date} 返回一个表示本地日期和时间的 Date 对象。

示例：
  
```
[js] view plaincopy
var dt = new Date();  
```  
  
console.log(dt); // => 返回一个表示本地日期和时间的 Date 对象  

#### new Date(milliseconds) ：把毫秒数转换为 Date 对象

参数：  

①milliseconds {int} ：毫秒数；表示从 '1970/01/01 00:00:00' 为起点，开始叠加的毫秒数。

注意：起点的时分秒还要加上当前所在的时区，北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00'

返回值：

{Date} 返回一个叠加后的 Date 对象。

示例：
  
```
[js] view plaincopy
var dt = new Date(1000 * 60 * 1); // 前进1分钟的毫秒数  
  
console.log(dt); // => {Date}:1970/01/01 08:01:00  
  
dt = new Date(-1000 * 60 * 1); // 倒退1分钟的毫秒数  
  
console.log(dt); // => {Date}:1970/01/01 07:59:00  
```  

#### new Date(dateStr) ：把字符串转换为 Date 对象
 
参数：  

①dateStr {string} ：可转换为 Date 对象的字符串(可省略时间)；字符串的格式主要有两种：

1) yyyy/MM/dd HH:mm:ss （推荐）：若省略时间，返回的 Date 对象的时间为 00:00:00。

2) yyyy-MM-dd HH:mm:ss ：若省略时间，返回的 Date 对象的时间为 08:00:00(加上本地时区)。若不省略时间，此字符串在IE中会转换失败!

返回值：

{Date} 返回一个转换后的 Date 对象。

示例：
  
```
[js] view plaincopy
var dt = new Date('2014/12/25'); // yyyy/MM/dd  
  
console.log(dt); // => {Date}:2014/12/25 00:00:00  
  
dt = new Date('2014/12/25 12:00:00'); // yyyy/MM/dd HH:mm:ss  
  
console.log(dt); // => {Date}:2014/12/25 12:00:00  
  
dt = new Date('2014-12-25'); // yyyy-MM-dd  
  
console.log(dt); // => {Date}:2014-12-25 08:00:00 (加上了东8区的时区)  
  
dt = new Date('2014-12-25 12:00:00'); // yyyy-MM-dd HH:mm:ss (注意：此转换方式在IE中会报错！)  
  
console.log(dt); // => {Date}:2014-12-25 12:00:00  
```  

#### new Date(year, month, opt\_day, opt\_hours, opt\_minutes, opt\_seconds, opt\_milliseconds) ：把年月日、时分秒转换为 Date 对象

参数
  
①year {int} ：年份；4位数字。如：1999、2014

②month {int} ：月份；2位数字。从0开始计算，0表示1月份、11表示12月份。

③opt\_day {int} 可选：号； 2位数字；从1开始计算，1表示1号。

④opt\_hours {int} 可选：时；2位数字；取值0~23。

⑤opt\_minutes {int} 可选：分；2位数字；取值0~59。

⑥opt\_seconds {int} 可选：秒；2未数字；取值0~59。

⑦opt\_milliseconds {int} 可选：毫秒；取值0~999。

返回值：

{Date} 返回一个转换后的Date对象。

示例：
  
```
[js] view plaincopy
var dt = new Date(2014, 11); // 2014年12月(这里输入的月份数字为11)  
  
console.log(dt); // => {Date}:2014/12/01 00:00:00  
  
dt = new Date(2014, 11, 25); // 2014年12月25日  
  
console.log(dt); // => {Date}:2014/12/25 00:00:00  
  
dt = new Date(2014, 11, 25, 15, 30, 40); // 2014年12月25日 15点30分40秒  
  
console.log(dt); // => {Date}:2014/12/25 15:30:40  
  
dt = new Date(2014, 12, 25); // 2014年13月25日(这里输入的月份数字为12，表示第13个月，跳转到第二年的1月)  
  
console.log(dt); // => {Date}:2015/01/25  
```  

### 属性

无；Date 对象对日期和时间的操作只能通过方法。

### 实例方法

　　Date 对象的实例方法主要分为2种形式：本地时间和 UTC 时间。同一个方法，一般都会有此2种时间格式操作(方法名带 UTC 的，就是操作 UTC 时间)，这里主要介绍对本地时间的操作。

#### get方法

##### getFullYear() ：返回 Date 对象的年份值；4位年份。
##### getMonth() ：返回 Date 对象的月份值。从0开始，所以真实月份=返回值+1 。

##### getDate() ：返回 Date 对象的月份中的日期值；值的范围1~31 。

##### getHours() ：返回 Date 对象的小时值。

##### getMinutes() ：返回 Date 对象的分钟值。

##### getSeconds() ：返回 Date 对象的秒数值。

##### getMilliseconds() ：返回 Date 对象的毫秒值。

##### getDay() ：返回 Date 对象的一周中的星期值；0为星期天，1为星期一、2为星期二，依此类推

##### getTime() ：返回 Date 对象与'1970/01/01 00:00:00'之间的毫秒值(北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00') 。

示例：
  
```
[js] view plaincopy
dt.getFullYear(); // => 2014：年  
  
dt.getMonth(); // => 11：月；实际为12月份(月份从0开始计算)  
  
dt.getDate(); // => 25：日  
  
dt.getHours(); // => 15：时  
  
dt.getMinutes(); // => 30：分  
  
dt.getSeconds(); // => 40：秒  
  
dt.getMilliseconds(); // => 333：毫秒  
  
dt.getDay(); // => 4：星期几的值  
  
dt.getTime(); // => 1419492640333 ：返回Date对象与'1970/01/01 00:00:00'之间的毫秒值(北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00')  
```  

####  set 方法

##### setFullYear(year, opt\_month, opt\_date) ：设置 Date 对象的年份值；4位年份。  

##### setMonth(month, opt\_date) ：设置 Date 对象的月份值。0表示1月，11表示12月。

##### setDate(date) ：设置 Date 对象的月份中的日期值；值的范围1~31 。

##### setHours(hour, opt\_min, opt\_sec, opt\_msec) ：设置 Date 对象的小时值。

##### setMinutes(min, opt\_sec, opt\_msec) ：设置 Date 对象的分钟值。

##### setSeconds(sec, opt\_msec) ：设置 Date 对象的秒数值。

##### setMilliseconds(msec) ：设置 Date 对象的毫秒值。

示例：
  
```
[js] view plaincopy
var dt = new Date();  
  
dt.setFullYear(2014); // => 2014：年  
  
dt.setMonth(11); // => 11：月；实际为12月份(月份从0开始计算)  
  
dt.setDate(25); // => 25：日  
  
dt.setHours(15); // => 15：时  
  
dt.setMinutes(30); // => 30：分  
  
dt.setSeconds(40); // => 40：秒  
  
dt.setMilliseconds(333); // => 333：毫秒  
  
console.log(dt); // =>  2014年12月25日 15点30分40秒 333毫秒  
```  

#### 其他方法

##### toString() ：将 Date 转换为一个'年月日 时分秒'字符串
##### toLocaleString() ：将 Date 转换为一个'年月日 时分秒'的本地格式字符串

##### toDateString() ：将 Date 转换为一个'年月日'字符串

##### toLocaleDateString() ：将 Date 转换为一个'年月日'的本地格式字符串

##### toTimeString() ：将 Date 转换为一个'时分秒'字符串

##### toLocaleTimeString() ：将 Date 转换为一个'时分秒'的本地格式字符串

##### valueOf() ：与getTime()一样， 返回 Date 对象与'1970/01/01 00:00:00'之间的毫秒值(北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00')

示例：
  
```
[js] view plaincopy
var dt = new Date();  
  
console.log(dt.toString()); // => Tue Dec 23 2014 22:56:11 GMT+0800 (中国标准时间) ：将Date转换为一个'年月日 时分秒'字符串  
  
console.log(dt.toLocaleString()); // => 2014年12月23日 下午10:56:11  ：将Date转换为一个'年月日 时分秒'的本地格式字符串  
  
console.log(dt.toDateString()); // => Tue Dec 23 2014 ：将Date转换为一个'年月日'字符串  
  
console.log(dt.toLocaleDateString()); // => 2014年12月23日 ：将Date转换为一个'年月日'的本地格式字符串  
  
console.log(dt.toTimeString()); // => 22:56:11 GMT+0800 (中国标准时间) ：将Date转换为一个'时分秒'字符串  
  
console.log(dt.toLocaleTimeString()); // => 下午10:56:11 ：将Date转换为一个'时分秒'的本地格式字符串  
  
console.log(dt.valueOf()); // => 返回Date对象与'1970/01/01 00:00:00'之间的毫秒值(北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00')  
```  

### 静态方法

#### Date.now()

说明：返回当前日期和时间的 Date 对象与'1970/01/01 00:00:00'之间的毫秒值(北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00')
参数：无

返回值：

{int} ：当前时间与起始时间之间的毫秒数。

示例：
  
```
[js] view plaincopy
console.log(Date.now()); // => 1419431519276  
```  

#### Date.parse(dateStr)

说明：把字符串转换为 Date 对象 ，然后返回此 Date 对象与'1970/01/01 00:00:00'之间的毫秒值(北京时间的时区为东8区，起点时间实际为：'1970/01/01 08:00:00')
参数：

①dateStr {string} ：可转换为 Date 对象的字符串(可省略时间)；字符串的格式主要有两种：

1) yyyy/MM/dd HH:mm:ss （推荐）：若省略时间，返回的 Date 对象的时间为 00:00:00。

2) yyyy-MM-dd HH:mm:ss ：若省略时间，返回的 Date 对象的时间为 08:00:00(加上本地时区)。若不省略时间，此字符串在IE中返回NaN(非数字)!

返回值：

{int} 返回转换后的Date对象与起始时间之间的毫秒数。

示例：
  
```
[js] view plaincopy
console.log(Date.parse('2014/12/25 12:00:00')); // => 1419480000000  
  
console.log(Date.parse('2014-12-25 12:00:00')); // => 1419480000000  (注意：此转换方式在IE中返回NaN！)  
``` 

### 实际操作

#### C# 的 DateTime 类型转换为 Js 的 Date 对象

说明：C# 的 DateTime 类型通过 Json 序列化返回给前台的格式为："\/Date(1419492640000)\/" 。中间的数字，表示 DateTime 的值与起始时间之间的毫秒数。
示例：

后台代码：简单的ashx
  
```
[js] view plaincopy
public void ProcessRequest (HttpContext context) {  
  
    System.Web.Script.Serialization.JavaScriptSerializer js = new System.Web.Script.Serialization.JavaScriptSerializer();  
  
    DateTime dt = DateTime.Parse("2014-12-25 15:30:40");  
  
    string rs = js.Serialize(dt); // 序列化成Json  
  
    context.Response.ContentType = "text/plain";  
  
    context.Response.Write(rs);  
  
}  
```  

前台代码：
  
```
[js] view plaincopy
var dateTimeJsonStr = '\/Date(1419492640000)\/'; // C# DateTime类型转换的Json格式  
  
var msecStr = dateTimeJsonStr.toString().replace(/\/Date\(([-]?\d+)\)\//gi, "$1"); // => '1419492640000' ：通过正则替换，获取毫秒字符串  
  
var msesInt = Number.parseInt(msecStr); // 毫秒字符串转换成数值  
  
var dt = new Date(msesInt); // 初始化Date对象  
  
console.log(dt.toLocaleString()); // => 2014年12月25日 下午3:30:40  
```  

#### 获取倒计时

说明：计算当前时间离目的时间相差多少天时分。
示例：
  
```
[js] view plaincopy
/** 
 
* 返回倒计时 
 
* @param dt {Date}：目的Date对象 
 
* @return {Strin} ：返回倒计时：X天X时X分 
 
*/  
  
function getDownTime(dt) {  
  
    // 1.获取倒计时  
  
    var intervalMsec = dt - Date.now(); // 目的时间减去现在的时间，获取两者相差的毫秒数  
  
    var intervalSec = intervalMsec / 1000; // 转换成秒数  
  
    var day = parseInt(intervalSec / 3600 / 24); // 天数  
  
    var hour = parseInt((intervalSec - day * 24 * 3600) / 3600); // 小时  
  
    var min = parseInt((intervalSec - day * 24 * 3600 - hour * 3600) / 60); // 分钟  
  
   
  
    // 2.若相差的毫秒小于0 ,表示目的时间小于当前时间，这时的取的值都是负的：-X天-时-分，显示时，只显示天数前面为负的就行。  
  
    if (intervalMsec < 0) {  
  
        hour = 0 - hour;  
  
        min = 0 - min;  
  
    }  
  
   
  
    // 3.拼接字符串并返回  
  
    var rs = day + '天' + hour + '时' + min + '分';  
  
    return rs;  
  
}  
  
   
  
// 当前时间：2014/12/28 13:26  
  
console.log(getDownTime(new Date('2015/06/01'))); // => 154天10时33分  
  
console.log(getDownTime(new Date('2014/01/01'))); // => -361天13时26分  
```  

#### 比较2个 Date 对象的大小

说明：可以对比2者的与起始时间的毫秒数，来区分大小。
示例：
  
```
[js] view plaincopy
var dt1 = new Date('2015/12/01');  
  
var dt2 = new Date('2015/12/25');  
  
console.log(dt1 > dt2); // => false   
```  