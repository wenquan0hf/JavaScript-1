# 浅谈 javascript 中自定义模版  
    
```
[js] view plaincopy
/** 
 * Created by Administrator on 15-1-19. 
 */  
function functionUtil() {  
}  
functionUtil = {  
  //某个DOM节点是否有某个属性  
  hasAttr: function (el, name) {  
    var attr = el.getAttributeNode && el.getAttributeNode(name);  
    return attr ? attr.specified : false  
  },  
  //根据class获取元素  
  getByClass: function (sClass, oParent) {  
    oParent = oParent || document;  
    if (!oParent.getElementsByClassName) {  
      return oParent.getElementsByClassName(sClass);  
    }  
    var arr = [];  
    var aEle = oParent.getElementsByTagName('*');  
    var reg = new RegExp('(^|\\s)' + sClass + '(\\s|$)');  
    //var reg = new RegExp('(^|[\\x20\\t\\r\\n\\f])' + sClass + '([\\x20\\t\\r\\n\\f]|$)');  
    for (var i = 0; i < aEle.length; i++) {  
      if (reg.test(aEle[i].className)) {  
        arr.push(aEle[i]);  
      }  
    }  
    return arr;  
  },  
  //动态添加样式表  
  addSheetFile: function (path) {  
    var fileref = document.createElement("link")  
    fileref.rel = "stylesheet";  
    fileref.type = "text/css";  
    fileref.href = path;  
    fileref.media = "screen";  
    var headobj = document.getElementsByTagName('head')[0];  
    headobj.appendChild(fileref);  
  },  
  //根据指定格式如 ${name} 绑定json数据  
  LoadJsonData: function (sParent, oJson) {  
    var oParent = document.getElementById(sParent);  
    if (oJson instanceof Array) {  
      var str = oParent.innerHTML;  
      for (var i = 0; i < oJson.length - 1; i++) {  
        oParent.innerHTML += str;  
      }  
      for (var d in oJson) {  
        oParent.children[d].innerHTML = oParent.children[d].innerHTML.replace(/\$\{(\w+)\}/g, function (str, $1) {  
          return oJson[d][$1] ? oJson[d][$1] : '';  
        });  
      }  
    } else {  
      oParent.innerHTML = oParent.innerHTML.replace(/\$\{(\w+)\}/g, function (str, $1) {  
        return oJson[$1] ? oJson[$1] : '';  
      });  
    }  
  },  
  //根据指定格式如<%……%>绑定json数据  
  TemplateEngine: function (html, options) {  
    html = html.replace(/(>)|(<)/g, function (str, $1, $2) {  
      switch (str) {  
        case $1:  
          return '>';  
        case $2:  
          return '<';  
      }  
    });  
    var re = /<%([^%>]+)?%>/g, reExp = /(^( )?(if|for|else|switch|case|break|{|}))(.*)?/g, code = 'var r=[];\n', cursor = 0;  
    var add = function (line, js) {  
      js ? (code += line.match(reExp) ? line + '\n' : 'r.push(' + line + ');\n') :  
        (code += line != '' ? 'r.push("' + line.replace(/"/g, '\\"') + '");\n' : '');  
      return add;  
    }  
    while (match = re.exec(html)) {  
      add(html.slice(cursor, match.index))(match[1], true);  
      cursor = match.index + match[0].length;  
    }  
    add(html.substr(cursor, html.length - cursor));  
    code += 'return r.join("");';  
    return new Function(code.replace(/[\r\t\n]/g, '')).apply(options);  
  }  
}  
```  
 
## 第一种方式:${key}

functionUtil.LoadJsonData(element, data);

”html“代码：  
  
```
[xhtml] view plaincopy
<div id="data">  
  <div class="item">  
    姓名：${name}<br/>  
    年龄：${age}<br/>  
    职业：${job}<br/><br/>  
  </div>  
</div>  
```  

javascript 代码:  
  
```
[js] view plaincopy
var data = [  
      {  
          name: '徐磊',  
          age: 24,  
          job: 'IT'  
        },  
        {  
          name: '李磊',  
          age: 23,  
          job: '翻译'  
        }  
  ];  
functionUtil.LoadJsonData('data', data);  
```

执行结果:
  
![](images/104.png)  

## 第二种方式 <% 代码 %>

functionUtil.TemplateEngine(string,Object);

"html"代码：  
  
```
[xhtml] view plaincopy
<div id="test3">  
  <%if(this.isShow){  
   for(var i in this.data){%>  
    <p href="#">姓名：<%this.data[i].name%></p>  
  
    <p href="#">年龄：<%this.data[i].age%></p>  
  
    <p href="#">工作：<%this.data[i].job%></p>  
    <br/>  
  <%}}%>  
</div>  
```

javascript代码：  
  
```
[js] view plaincopy
var person = {  
        data: [  
          {  
            name: '徐磊',  
            age: 24,  
            job: 'IT'  
          },  
          {  
            name: '李磊',  
            age: 23,  
            job: '翻译'  
          }  
        ],  
        isShow: true  
      }  
  document.getElementById("test3").innerHTML = functionUtil.TemplateEngine(document.getElementById("test3").innerHTML, person);  
``` 

结果：
  
![](images/105.png) 

以上就是本文的全部内容了，小伙伴们看完是否对 javascript 模板有了新的认识了呢，希望大家能够喜欢。