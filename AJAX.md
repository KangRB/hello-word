<h1 align="center">AJAX--笔记</h1>
## Asynchronous Javascript And Xml

# 目录
1. [服务器常用状态码及其含义](#user-content-服务器常用状态码及其含义)
2. [回调函数](#user-content-回调函数)

***

# 服务器常用状态码及其含义

  + 100——客户必须继续发出请求
  + 101——客户要求服务器根据请求转换HTTP协议版本
  + 200——交易成功
  + 201——提示知道新文件的URL
  + 202——接受和处理、但处理未完成
  + 203——返回信息不确定或不完整
  + 204——请求收到，但返回信息为空
  + 205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
  + 206——服务器已经完成了部分用户的GET请求
  + 300——请求的资源可在多处得到
  + 301——删除请求数据
  + 302——在其他地址发现了请求数据
  + 303——建议客户访问其他URL或访问方式
  + 304——客户端已经执行了GET，但文件未变化
  + 305——请求的资源必须从服务器指定的地址得到
  + 306——前一版本HTTP中使用的代码，现行版本中不再使用
  + 307——申明请求的资源临时性删除
  + 400——错误请求，如语法错误
  + 401——请求授权失败
  + 402——保留有效ChargeTo头响应
  + 403——请求不允许
  + 404——没有发现文件、查询或URl
  + 405——用户在Request-Line字段定义的方法不允许
  + 406——根据用户发送的Accept拖，请求资源不可访问
  + 407——类似401，用户必须首先在代理服务器上得到授权
  + 408——客户端没有在用户指定的饿时间内完成请求
  + 409——对当前资源状态，请求不能完成
  + 410——服务器上不再有此资源且无进一步的参考地址
  + 411——服务器拒绝用户定义的Content-Length属性请求
  + 412——一个或多个请求头字段在当前请求中错误
  + 413——请求的资源大于服务器允许的大小
  + 414——请求的资源URL长于服务器允许的长度
  + 415——请求资源不支持请求项目格式
  + 416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
  + 417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求
  + 500——服务器产生内部错误
  + 501——服务器不支持请求的函数
  + 502——服务器暂时不可用，有时是为了防止发生系统过载
  + 503——服务器过载或暂停维修
  + 504——关口过载，服务器使用另一个关口或服务来响应用户，等待时间设定值较长
  + 505——服务器不支持或拒绝支请求头中指定的HTTP版本

  <h3 align="right"><a href="#user-content-AJAX--笔记">返回目录</a></h3>

  ***

# 回调函数
```
//第一步：创建xhr对象
var xhr = null;
if(window.XMLHttpRequest){//标准浏览 器
  xhr = new XMLHttpRequest();
}else{
  xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
var username = document.querySelector('#username').value;
var password = document.querySelector('#password').value;

//第二步：准备发送请求-配置发送请求的一些行为
url = 'demo.php?username='+username+'&password='+password;
xhr.open('get',url,true);
//第一个参数表示用什么方式发送请求，第二个参数表示往哪儿发送请求，第三个参数表示同步(false)还是异步(true默认)

//第三步：执行发送的请求
xhr.send(null);

//第四步：指定一些回调函数
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4){
    if(xhr.status == 200){
      //200 表示服务器数据发送过来了并且已被浏览者看到
      var data = xhr.responseText;//json
      // var data = xhr.responseXML;
    }
  }
}

demo.php
<?php
  $username = #_GET['username'];
  $password = #_GET['password'];

  echo '用户名：'.$username.'密码：'.$password;
?>
```
### xhr.readyState的状态
+ 0：XMLHttpRequest 对象创建完成
+ 1：表示发送请求的动作准备好了，但是还没有开始发送
+ 2：表示已经发送完成
+ 3：服务器已经返回了数据
+ 4：服务器返回的数据已经可以使用

<h3 align="right"><a href="#user-content-AJAX--笔记">返回目录</a></h3>

***

# JSON
>JSON实际上是JavaScript的一个子集，所以JSON的数据格式和JavaScript是对应的
```
number  => JS number
boolean => JS boolean
string  => JS string
null    => JS null
array   => JS Array 的表达方式[]
object  => JS Object {}表达式
```
+ JSON规定字符集是UTF-8，字符串必须用""，Object的键也必须使用""
+ 数组或对象的最后一个成员，不能加 , 逗号

### JS内置的两个JSON方法
1. JSON.stringify({},[],""); //json对象转换为json字符串
    1. 参数1：要序列化的数据（object格式）
    2. 参数2：控制对象的键值，只想输出指定的属性，传入一个数组
    3. 参数3：序列化后，打印输出的格式（一个Tab，可以更直观查看json）
>任何吧JavaScript变成Json，就是把这个对象序列化为Json字符串，然后才可以通过网络传递

2. JSON.parst(json.data); //传入的是json字符串
>如果收到了一个JSON格式的字符串，只需要把它反序列化为一个JavaScript对象，就可以在JavaScript中直接使用这个对象了

### 实例
1. 数组方式[]
```js
[{"id" : 1,
"name" : "name1"
},{
"id" : 2,
"name" : "name2"
}]
```
2. 字符串方式''
```js
'{{"id" : 1,"name" : "name1"},{"id" : 2,"name" : "name2"}}'
```
3. 对象方式{}
```js
{
  "status" : 0,      //执行状态码
  "msg" : "SUCCESS", //说明文字信息，没有为NULL
  "data" : [{        //对象中嵌套数组，数组是返回数据  
    "id" : 1,
    "name" : "name1"
    },{
    "id" : 2,
    "name" : "name2"
    }]
}
```

<h3 align="right"><a href="#user-content-AJAX--笔记">返回目录</a></h3>

***

# jQuery的AJAX语法
```js
$.ajax({
  type:'',//POST | GET 默认值为GET
  url:'',//请求的地址 ，默认值为当前页地址
  dataType:'',//xml,html,script,json,jsonp,text
  jsonp:"_jsonp",//传递给请求处理程序或页面的，可以获得jsonp回调函数名的参数名(默认为callback)
  jsonCallback:"",//自定义的json回调函数名称，默认为jQuery自动生成的随机函数名
  success:function(data){
    //请求成功后的回调函数
  },
  error:function(e){
    //请求失败时的回调函数
  }
});
```
