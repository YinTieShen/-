# Ajax知识点梳理

标签（空格分隔）： ajax Node.js express

---
###**第二章：搭建Node服务器**
----------
####**2.1 使用**

>1.配置Node环境 node -v; npm -v;
2.新建文件 test.js => cmd 路径 node test.js
```
console.log("hello");
```


----------


####**2.2 Node搭建服务器**
>浏览器查看：http://localhost：3000 
```
var http = require('http');
http.createServer(function(req,res){
    res.writeHead(200, {'Content-Type': 'text/plain;charset=utf-8'});
    res.end("我是小小服务器");
}).listen(3000);
```
---
###**第三章：搭建express框架**
---

####**1. 安装supervisor工具**
>问题： 每次修改文件，刷新页面不能生效；
>安装： npm install supervisor -g：全局安装

####**2. 使用express框架**
>1.下载express框架

    1. npm install express-generator -g (安装express的工具)
    2. express 项目名称 -e（自动化创建项目）
    3. 进入package.json文件中:"start" : "node ./bin/www"  => "start" : "supervisor ./bin/www" 
         
>2.执行 npm install；

>3.目录结构：
```
- bin：命令执行目录
- public ： 公共目录，存放CSS，JS，图片，音视频等资源
- routes ：路由文件目录
- views ： 存放模板目录
- app.js ：项目的启动文件
- package ： 项目的说明文件
```
> *4.* npm start
> 5. 修改app.js 中的代码（15行）
```
// view engine setup
app.set('views', path.join(__dirname, 'views'));
var ejs = require('ejs');
app.engine('html', ejs.__express);
app.set('view engine', 'html');
```
6.修改views目录下的文件后缀名为html

---
>routes文件夹：存放路由文件
```
index.js //路由作用：请求方式+请求的路径
var express = require('express');
var router = express.Router();

/* GET home page. */
//网站路由
router.get('/', function(req, res, next) {
    res.render('index', { title: 'Express' });
});

router.get('/list',function(req,res,next){
  res.render('list', { title: '清单页面' });
})

module.exports = router;
```
>使用express实现一个登陆页面post请求
```
login.js
<body>
    <form action="dologin" method="GET">
            用户名：<input type="text" name="username" value=""><br>
            密码：<input type="password" name="password" value=""><br>
            <input type="submit" value="请提交""><br>
    </form>
</body>
```
> put接受信息用 req.body
```
index.js
//登录页面路由
router.get('/login',function(req,res){
  res.render('login');
})
//post提交路由设置
router.post('/dologin',function(req,res){ //post提交
  console.log(req.body);//接受input框传入的信息
  res.end("login success");
})
//put接受信息用req.body
//get接受信息用req.query
router.get('/dologin2',function(req,res){
  console.log(req.query);
  res.send('login success');
})
```
>get接受URL地址中参数信息方式：`req.query` , `req.parmas`;
```
//第一种方式：localhost：3000/test?usernmae=xiaoming&age=18&...
router.get('/test',function(req,res){
  console.log(req.query);
  res.send('test success');
})
//第二种方(更有利于SEO)：localhost：3000/demo/Jeff/18/man
router.get('/test1/:user/:age/:sex',function(req,res){
  console.log(req.params);
  res.send('test1 success');
})
```
>路由响应信息：`res.render` , `res.end` , `res.json` , `res.send` ;
```
//响应json数据
router.get('/json',function(req,res){
  var info = {user:"刘军军",age:18,sex:"female"};
  res.json(info);
  //多组数据：
  var data = [
        {user:"刘军军",age:18,sex:"female"},
        {user:"小高",age:18,sex:"male"}，
        {user:"吴亦凡",age:18,sex:"female"}，
        {user:"郭德纲",age:18,sex:"female"}
    ];
    res.json(data);
})
```
---
###**第三章：揭开ajax的神秘面纱**
---
####**1.Ajax(Asynchronous JavaScript and XML):异步JS和XML技术**

> **1.**背景

1.传统的Web网站，提交表单，需要重新加载整个页面。

2.如果服务器长时间未能返回Response，则客户端将会无响应，用户体验很差。

3.服务端返回Response后，浏览器需要加载整个页面，对浏览器的负担也是很大的。

4.浏览器提交表单后，发送的数据量大，造成网络的性能问题。

>**2.** Ajax

1.AJAX = 异步 JavaScript 和 XML。

2.AJAX 是一种用于创建快速动态网页的技术。

3.通过在后台与服务器进行少量数据交换，可以使网页实现异步更新。

4.可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

>**3.**前后端数据交互：

        JSON、 String、 XML


>**4.**原理：

        我理解的Ajax像是一个秘书一样，当浏览器需要与后台进行数据交互时，不需要再直接发送请求，而是创建一个XMLHTTPRequest对象，通过此对象将请求数据发送到远程服务器，此时浏览器可同时进行别的任务，后台将结果返回给XML对象后，再由该对象与浏览器进行交互。
        
        
![Ajax原理][1]
>**5.**原生Ajax请求
```
--Ajax封装
    function Ajax(dataType,bool){
        var ajax = new Object;
        ajax.xhr = new  XMLHttpRequest(); 
        ajax.type = dataType == undefined ? 'HTML' : dataType;
        ajax.callback;
        ajax.bool = bool == undifined ? true:bool;
        ajax.change = function(){
            if(xhr.readyState==4 && xhr.status == 200){
                if(ajax.type == 'HTML'){
                    ajax.callback(ajax.xhr.responseText);
                }else if(ajax.type == 'json'){
                    var obj = JSON.parse(ajax.xhr.responseText());
                    ajax.callback(obj);
                }
            }
        }
        ajax.get = function(url,callback){
            ajax.callback = callback;
            ajax.xhr.onreadystatechange = ajax.change;
            ajax.xhr.open('GET',url,ajax.bool);
            ajax.xhr.send();
        } 
        ajax.post = function(url,data,callback){
            ajax.callback = callback;
            ajax.xhr.onreadystatechange = ajax.change;
            ajax.xhr.open('POST',url,ajax.bool);
            xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
            xhr.send(data);
        }
        return ajax；
    }
```
>XMLHttpRequest对象
    
    1.属性：
当需要异步与服务器交换数据时，需要XMLHttpRequest对象来异步交换。XMLHttpRequest对象的主要属性有：

    onreadystatechange ： 每次状态改变所触发事件的事件处理程序。
    responseText ： 从服务器进程返回数据的字符串形式。
    responseXML ： 从服务器进程返回的DOM兼容的文档数据对象。
    status ： 从服务器返回的数字代码，如404（未找到）和200（已就绪）。
    status Text ： 伴随状态码的字符串信息。
    readyState ： 对象状态值。对象状态值有以下几个：
```
0 - (未初始化)还没有调用send()方法
1 - (载入)已调用send()方法，正在发送请求
2 - (载入完成)send()方法执行完成
3 - (交互)正在解析响应内容
4 - (完成)响应内容解析完成，可以在客户端调用了
```
    2.方法
XMLHttpRequest对象有两个重要方法 open与send。
对于open方法，有几点需要注意：

    URL是相对于当前页面的路径，也可以用绝对路径。
    
    open方法不会向服务器发送真正请求，它相当于初始化请求并准备发送。
    
    只能向同一个域中使用相同协议和端口的URL发送请求，否则会因为安全原因报错。
    
    真正能够向服务器发送请求需要调用send方法，并仅在POST请求可以传入参数，不需要则发送null，在调用send方法之后请求被发往服务器。  

>GET 还是 POST？

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：
```
无法使用缓存文件（更新服务器上的文件或数据库）
向服务器发送大量数据（POST 没有数据量限制）
发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠
```
>Ajax缺点：

它可能破坏浏览器的后退与加入收藏书签功能。

        关于浏览器回退，HTML5之前的方法大多是在用户单击后退按钮访问历史记录时，通过创建或使用一个隐藏的IFRAME来重现页面上的变更。（例如，当用户在Google Maps中单击后退时，它在一个隐藏的IFRAME中进行搜索，然后将搜索结果反映到Ajax元素上，以便将应用程序状态恢复到当时的状态）。
        
        关于无法将状态加入收藏或书签的问题，HTML5之前的一种方式是使用URL片断标识符（通常被称为锚点，即URL中#后面的部分）来保持追踪，允许用户回到指定的某个应用程序状态。（许多浏览器允许JavaScript动态更新锚点，这使得Ajax应用程序能够在更新显示内容的同时更新锚点。）HTML5以后可以直接操作浏览历史，并以字符串形式存储网页状态，将网页加入网页收藏夹或书签时状态会被隐形地保留。上述两个方法也可以同时解决无法后退的问题。
```
//ajax get请求实现向后台传递参数接受并返回
-ajaxtest.html //
<h1>学习ajax请求</h1>
    <button>点击发送get请求</button>
    <script>
        var btn = document.getElementsByTagName('button')[0];
        btn.onclick = function(){
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function(){
                if(xhr.readyState==4 && xhr.status == 200){
                    alert(xhr.responseText);
                }
            }
            xhr.open('GET','/get?user=郭德纲',true);
            xhr.send();
        }
    </script>
-index.js
//首先配置ajaxtest.html路由
router.get('/ajaxtest',function(req,res){
  res.render('ajaxtest');
})
//接受路径 接收参数用req.query.user;
router.get('/get',function(req,res){
  var user = req.query.user;
  res.end('姓名是'+user);
})
//get传参是直接在写入url
//而post则在send中传递参数，且post必须设置头信息，
```

    ajax获取json数据
```
在onreadystatechange内设置var obj = JSON.parse(xhr.responseText);
路由设置json数据
router.post('/json1',function(req,res){
  var info = {
    name:"黑大帅",
    age:18,
    sex:"nan"
  }
  res.json(info);
})
```
>ajax实现用户名是否存在验证
```
<input type="text" id="name">
<button>点击发送post请求</button>
```
```javascript
var btn = document.getElementsByTagName('button')[0];
btn.onclick = function(){
    var name = document.getElementById("name").value;
    Ajax().post('/check','user='+name,function(msg){
        alert(msg);
    })
}

//路由端
router.post('/check',function(req,res){
  var user = req.body.user;
  console.log(user);
  var data = ['zhangsan','lisi','wangwu'];
  //res.end(user);
  if(data.indexOf(user) != -1){
    res.end("用户名已存在");
  }else{
    res.end("用户名可以使用");
  }
})
```
>####**Ajax跨域**

[浏览器同源政策及其规避方法][2]

[跨域资源共享 CORS 详解][3]

    域：协议名+主机名+端口号
    
    同源策略：安全问题 防止CSRF攻击

跨域处理：

>#####**jsonp**

当前的script脚本中定义函数,引入的js文件中调用函数

    JSONP之所以能够用来解决跨域方案,主要是因为 <script> 脚本拥有跨域能力,而JSONP正是利用这一点来实现.

1.客户端网页网页通过动态添加一个<script>元素，向服务器请求JSON数据，这种做法不受同源政策限制。
```
function addScriptTag(src) {
  var script = document.createElement('script');
  script.setAttribute("type","text/javascript");
  script.src = src;
  document.body.appendChild(script);
}

window.onload = function () {
  addScriptTag('http://example.com/ip?callback=foo');
}

function foo(data) {
  console.log('response data: ' + JSON.stringify(data));
};                      
    
```
由于<script>元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了foo函数，该函数就会立即调用。作为参数的JSON数据被视为JavaScript对象，而不是字符串，因此避免了使用JSON.parse的步骤。

    使用注意
    
JSONP只能是“GET”请求,不能进行较为复杂的POST和其它请求,而且只能返回javaScript代码段，需要进行封装调用。


----------


>#####**CORS**

    CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。
        整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

>因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

#####Express实现CROS跨域访问

    1. 建立两个express项目（client、server）
    2.一个设置端口3000，一个设置端口3001
    3.同时开启两个npm

```
//clinet(index.html) 端口号为3001

<button id="btn">点击发送跨域请求</button>
<script>
      var btn = document.getElementById('btn');
      btn.onclick = function(){
        Ajax().get("http://localhost:3000/json1",function(meg){
            console.log(meg);
        })
      }
</script>

//Server 先部署CROS头文件，后设置路由
//部署头文件 设置跨域访问
var express = require('express');
var router = express.Router();

router.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

//设置路由
router.get('/json1',function(req,res){
  var info = {
    name:"黑大帅",
    age:18,
    sex:"nan"
  }
  res.json(info);
})

```

  [1]: http://www.th7.cn/d/file/p/2015/09/21/7e175333861c3be3aaa4f55940a582af.jpg
  [2]: http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html
  [3]: http://www.ruanyifeng.com/blog/2016/04/cors.html