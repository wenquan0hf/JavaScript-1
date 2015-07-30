# 浅谈 Javascript Base64 加密解密  
  
html代码：
  
```
[js] view plaincopy
<!DOCTYPE html>  
  
 <html>  
  
 <head>  
  
     <title>Page Title</title>  
  
     <style type="text/css">  
  
     *{font-family: Consolas;font-style: italic}  
  
     .responsebox{width:900px;margin:10px auto;padding:10px;border:2px solid #366;border-radius: 10px 0 10px 0; text-align: center}  
  
     .responsebox input,.responsebox button{font-size: 30px;margin:5px;padding:5px;}  
  
     .spansuper{vertical-align: super;font-size: 14px}  
  
     .spanbottom{vertical-align: text-bottom;font-size: 12px;margin-left: -110px}  
 
     #showbox{width:900px;height:430px;border:5px solid #663;border-radius: 0 20px 0 20px;margin:10px auto;padding:8px;font-size: 20px}  
  
     </style>  
  
 </head>  
  
 <body>  
  
 <div class="responsebox">  
  
     <h1>Javascript Base64 Encode & Decode<span class="spansuper">veinyf@gmail.com</span><span class="spanbottom">2014-12-27 17:44</span></h1>  
  
     <input type="text" id="input">  
  
     <input type="checkbox" id="checkbox" checked="checked">Base64</input>  
  
     <button id="btn">Convert done !</button>  
  
 </div>  
  
 <div id="showbox"></div>  
  
 </body>  
  
 <script type="text/javascript">  
  
     /*javascript知识： 
 
      *函数：window.atob()    window.btoa()   unescape() escape() encodeURIComponent() decodeURIComponent() 
 
      *正则表达式清除首位空格：_string.replace(/(^\s*)|(\s*$)/g,""); 
 
      * 
 
      *CovertBase64orString自执行函数 
 
      *inputid   输入框id 
 
      *checkboxid    选择框id 
 
      *btnid 按钮id 
 
      *showid    html显示容器id，这里是一个div#showbox 
 
      */  
  
 (function CovertBase64orString(inputid, checkboxid, btnid, showid) {  
  
     var checkbox = document.getElementById(checkboxid); //html dom select checkbox  
  
     var chkvalue = checkbox.getAttribute("checked");    //html dom select checkedvalue  
  
     var btn = document.getElementById(btnid);           //html dom select button id  
  
     var isbase64;                                       //base64toString or StringtoBase64 bool  
  
     var returnval = null;                               //Converted string  
  
     chkvalue == "checked" ? isbase64 = true : isbase64 = false; //判断check按钮初始化状态 赋值isbase64  
  
     checkbox.addEventListener("click", function(e) {            //checkbox 点击事件注册  
  
         var _ckvak = checkbox.getAttribute("checked");          //点击事件发生时，改变check状态，赋值isbase64  
  
         if (_ckvak == "checked") {  
  
             checkbox.setAttribute("checked", null);  
  
             isbase64 = false;  
  
         } else {  
  
             checkbox.setAttribute("checked", "checked");  
  
             isbase64 = true;  
  
         }  
  
     }, true);  
  
     btn.addEventListener("click", function(e) {                    //button 点击事件注册  
  
         var _show = document.getElementById(showid);               //html dom select showbox id  
  
         var _inputvalue = document.getElementById(inputid).value;   //文本框取值  
  
         //_inputvalue=_inputvalue.replace(/(^\s*)|(\s*$)/g, "");    //正则表达式去除首位空格，似乎btoa，abob已经做了这些工作  
  
         var _showlength = _show.childNodes.length;                  //遍历showbox，清除showbox内容  
  
         while (_showlength > 0) {  
  
             _show.removeChild(_show.childNodes[_showlength - 1]);  
  
             _showlength--;  
  
         }  
  
         if (isbase64) {  //string to base64，支持中文编码，unescape，encodeURIComponent  
  
             returnval = window.btoa(unescape(encodeURIComponent(_inputvalue)));  
  
         } else {        //base64 to string  
  
             returnval = decodeURIComponent(escape(window.atob(_inputvalue)));  
  
         }  
  
         _show.appendChild(document.createTextNode(returnval));          //add context to showbox  
  
     }, true);  
  
 })("input", "checkbox", "btn","showbox");  
  
 //CovertBase64orString("input", "checkbox", "btn","showbox");  
  
 </script>  
  
 </html>  
```  

效果：

![](images/101.png)  

推荐一个 Javascript IDE 比Aptana 还好用。Komodo IDE（免费版：Komodo Edit，基本功能一样）支持语法高亮，智能感知，还支持 perl，python，ruby，nodejs 语法等  
  
![](images/102.png)