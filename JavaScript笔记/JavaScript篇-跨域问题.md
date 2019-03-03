
什么是跨域
---
跨域是指从一个域名的网页去请求另一个域名的资源。比如从www.baidu.com 页面去请求 www.google.com 的资源。但是一般情况下不能这么做，它是由浏览器的同源策略造成的，是浏览器对JavaScript施加的安全限制。

严格一点来说：只要 协议，域名，端口有任何一个的不同，就被当作是跨域

http://www.123.com/index.html 调用 http://www.123.com/server.PHP （非跨域）

http://www.123.com/index.html 调用 http://www.456.com/server.php （主域名不同:123/456，跨域）

http://abc.123.com/index.html 调用 http://def.123.com/server.php （子域名不同:abc/def，跨域）

http://www.123.com:8080/index.html 调用 http://www.123.com:8081/server.php （端口不同:8080/8081，跨域）

http://www.123.com/index.html 调用 https://www.123.com/server.php （协议不同:http/https，跨域）

请注意：localhost和127.0.0.1虽然都指向本机，但也属于跨域。

怎么解决跨域
---
主要的5种方法
* 跨域资源共享（CORS）
* 通过jsonp跨域
* 通过修改document.domain来跨子域
* 使用window.name来进行跨域
* 使用HTML5的window.postMessage方法跨域

具体实现方法
---
# 跨域资源共享（CORS）
原理：CORS的思想，就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。
 ```
  在服务器端，通过设置Access-Control-Allow-Origin来允许域请求
  如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。
   'Access-Control-Allow-Origin:*' //设置请求来源不受限制或者可以直接指定域名
 ```
# 通过jsonp跨域
jsonp，即json with padding，顾名思义，就是将json填充到一个盒子里

原理：通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入，所以jsonp是需要服务器端的页面进行相应的配合的

情景：有个a.html页面，它里面的代码需要利用ajax获取一个不同域上的json数据，假设这个json数据地址是http://example.com/data.php
```
  // a.html
  
  <script type="text/javascript">
      function dosomething(jsondata){
          //处理获得的json数据
      }
  </script>
  <script src="http://example.com/data.php?callback=dosomething"></script>
```
```
  // 服务端
  
  <?php
    $callback = $_GET['callback'];//得到回调函数名
    $data = array('a','b','c');//要返回的数据
    echo $callback.'('.json_encode($data).')';//输出
  ?>
```
输出结果：dosomething(['a','b','c'])

# 通过修改document.domain来跨子域（只适用于不同子域的框架间的交互）
情景：有一个页面，地址是http://www.example.com/a.html ，在这个页面里面有一个iframe，它的src是http://example.com/b.html, 很显然，这个页面与它里面的iframe框架是不同域的，所以我们是无法通过在页面中书写js代码来获取iframe中的东西的

```
//  http://www.example.com/a.html页面

  <iframe id = "iframe" src="http://example.com/b.html" onload = "test()"></iframe>
  <script type="text/javascript">
      document.domain = 'example.com';//设置成主域
      function test(){
          alert(document.getElementById('￼iframe').contentWindow);//contentWindow 可取得子窗口的 window 对象
      }
  </script>
```

```
  //  http://example.com/b.html页面
  
  <script type="text/javascript">
      document.domain = 'example.com';//在iframe载入这个页面也设置document.domain，使之与主页面的document.domain相同
  </script>
```
# 使用window.name来进行跨域

情景：有一个www.example.com/a.html页面,需要通过a.html页面里的js来获取另一个位于不同域上的页面www.cnblogs.com/data.html里的数据。

```
  // data.html页面
  
  <script>
    window.name = '我是a.html页面想要的数据';
  </script>
```
那么在a.html页面中，我们怎么把data.html页面载入进来呢？显然我们不能直接在a.html页面中通过改变window.location来载入data.html页面，因为我们想要即使a.html页面不跳转也能得到data.html里的数据。答案就是在a.html页面中使用一个隐藏的iframe来充当一个中间人角色，由iframe去获取data.html的数据，然后a.html再去得到iframe获取到的数据。

充当中间人的iframe想要获取到data.html的通过window.name设置的数据，只需要把这个iframe的src设为www.cnblogs.com/data.html就行了。然后a.html想要得到iframe所获取到的数据，也就是想要得到iframe的window.name的值，还必须把这个iframe的src设成跟a.html页面同一个域才行，不然根据前面讲的同源策略，a.html是不能访问到iframe里的window.name属性的。这就是整个跨域过程。

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <script>
    function getData(){            // iframe 载入data.html页面后会执行此函数
      var iframe = document.getElementById('proxy');
      iframe.onload = function(){    // 这个时候a.html与iframe已经是处于同源，可以互相访问
        var data = iframe.contentWindow.name;
        alert(data);
       }
       iframe.src = 'b.html';   // 这里的b.html为随便的一个页面，只要与a.html同源就行，目的是让a.html能访问到iframe里的东西
    }
  </script>
</head>
<body>
  <iframe id="proxy" src="http://www.cnblogs.com/data.html" style="display:none" onload="getData()"></iframe>
</body>
</html>
```
#  使用HTML5的window.postMessage方法跨域

该方法是html5新引进的特性，可以使用它来向其它的window对象发送消息，无论这个window对象是属于同源或不同源

用postMessage方法的window对象是指要接收消息的那一个window对象，该方法的第一个参数message为要发送的消息，类型只能为字符串；第二个参数targetOrigin用来限定接收消息的那个window对象所在的域，如果不想限定域，可以使用通配符 * 

看下面的例子，两个页面
```
   //  http://test.com/a.html
   
   <script>
      function onLoad(){
        var iframe = document.getElementById('iframe');
        var win = iframe.contentWindow;
        win.postMessage('哈哈，我是来自页面a的消息','*');
      }
   </script>
   <iframe id="iframe" src="http://www.test.com/b.html" onload="onLoad()"> </iframe> 
  
```

```
  //  http://www.test.com/b.html 页面
  
  <script>
    window.onmessage = function(e){
        e=e||event;
        alert(e.data);
    }
  </script>

```

参照博客
---

https://www.cnblogs.com/yongshaoye/p/7423881.html









