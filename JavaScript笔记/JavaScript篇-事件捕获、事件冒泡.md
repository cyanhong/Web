事件捕获和事件冒泡
---

### 先来看看基本概念
```
事件捕获：从window一直到当前点击的元素，即从上往下
事件冒泡：顾名思义，往上冒泡，所以是从当前点击的元素一直到window
```

通过今天的面试，总结出光了解概念是不行的，还是得自己动手写代码

```
  <div id="obj1">我是第一
      <div id="obj2">我是第二
        <a id="obj3" href="https://www.baidu.com/">我是第三</a>
      </div>
   </div>
   
   //事件捕获  从最不具体的元素到最具体的
   <script type="text/javascript">
      var obj1=document.getElementById("obj1");
      var obj2=document.getElementById("obj2");
      var obj3=document.getElementById("obj3");
      obj1.addEventListener("click",function(){
        alert("我是obj1")
      },true);     
      obj2.addEventListener("click",function(){
        alert("我是obj2")
      },true);
      obj3.addEventListener("click",function(){
        alert("我是obj3")
      },true);
   </script>

//点击我是第一，弹出 我是obj1
//点击我是第二，先弹出 我是obj1，再弹出 我是obj2
//点击我是第三，先弹出 我是obj1，再弹出 我是obj2，再弹出我是obj3，最后跳到百度主页
```
```
 //事件冒泡 从最具体的元素到最不具体的
   <script type="text/javascript">
      var obj1=document.getElementById("obj1");
      var obj2=document.getElementById("obj2");
      var obj3=document.getElementById("obj3");
      obj1.addEventListener("click",function(){
        alert("我是obj1")
      });     //第三个参数默认是false   
      obj2.addEventListener("click",function(){
        alert("我是obj2")
      });
      obj3.addEventListener("click",function(){
        alert("我是obj3")
      });
   </script>

//点击我是第一，弹出 我是obj1
//点击我是第二，先弹出 我是obj2，再弹出 我是obj1
//点击我是第三，先弹出 我是obj3，再弹出 我是obj2，再弹出我是obj1，最后跳到百度主页
```

### 有的时候我们并不想它发生事件冒泡，所以怎么阻止呢

方法1 `e.stopPropagation`
```
  obj3.addEventListener("click",function(e){
    	e.stopPropagation();
  });
  //现在点击 我是第三 不弹出任何信息，直接跳到百度主页
```
方法2 `e.preventDefault`
```
  obj3.addEventListener("click",function(e){   	
    	alert("我是obj3");
    	e.preventDefault(); 
    });
    // 点击我是第三，依次弹出 我是obj3 我是obj2 我是obj1，但是不跳转
```
方法3 `return false`
```
  <script src="http://libs.baidu.com/jquery/1.9.0/jquery.js"></script>
  
   $("#obj3").on("click",function(e){
    	alert("我是obj3");
    	return false;
    })
    // 点击我是第三，弹出 我是obj3，不跳转
```
注意return false必须使用jquery才能阻止冒泡和默认行为

通过上面三种方法对比，可以得出

* e.stopPropagation 阻止冒泡事件，不阻止默认行为
* e.preventDefault 不阻止冒泡事件，阻止默认行为
* return false 既阻止冒泡事件，又阻止默认行为

事件委托
---

现在假设有一个ul下面有两个li标签，要实现点击li标签，弹出对应的内容,我们可以这样写：
```
  <ul id="ull">
		<li id="obj1">1111</li>
		<li id="obj2">2222</li>
	</ul>
  
  var ull=document.getElementById("ull");
  var obj1=document.getElementById("obj1");
  var obj2=document.getElementById("obj2");
  obj1.addEventListener("click",function(e){
    alert("1111")
  });
  obj2.addEventListener("click",function(e){
    alert("2222")
  });
```
但是问题来了，如果我有1万个li标签，难道要写1万个点击事件,操作1万次DOM，这不切实际，性能不允许

这时候事件委托来了
```
  ull.addEventListener("click",function(e){
    var x=e.target;    //获取当前事件操作的DOM
    if(x.nodeName.toLowerCase()==='li'){    //nodeName获取当前标签名
      switch(x.id){
        case: 'obj1':
          alert(1111);
          break;
        case: 'obj2':
          alert(2222);
          break;
     }
   }
   // 因为事件冒泡，所以点击li标签，会冒泡到ul标签，从而只用一次DOM操作就能完成所有效果，性能相比上面有较大提升
```

### 考虑新增节点

```
  <input type="button" id="btn" value="添加" />
	<ul id="ull">nnn
		<li id="obj1">1111</li>
		<li id="obj2">2222</li>
	</ul>
  
  var btn=document.getElementById("btn");
  var ull=document.getElementById("ull");
  var lii=document.getElementsByTagName("li");
  //鼠标移入背景变红，移除背景变白
  for(var i=0;i<lii.length;i++){
  	lii[i].onmouseover=function(){
  		this.style.background= "red";
  	};
  	lii[i].onmouseout=function(){
  		this.style.background= "#fff";
  	}
  }
  //点击添加按钮就增加一个li标签
  btn.addEventListener("click",function(){
  	var Li = document.createElement("li");
  	ull.appendChild(Li);
  });
```
上图实现了一个点击按钮，就新增标签，但新增的标签并没有事件啊，没有达到鼠标移入变红，移除变白的效果

所以我们继续
```
  function Hover(){
      for(var i=0;i<lii.length;i++){
        lii[i].onmouseover=function(){
          this.style.background= "red";
        };
        lii[i].onmouseout=function(){
          this.style.background= "#fff";
        }
      }
  }
  Hover();
  btn.addEventListener("click",function(){
    var Li = document.createElement("li");
    ull.appendChild(Li);
    Hover();
  });
```
此时效果达到了，但问题是这又增加了一次DOM操作啊，emmm 事件委托又来了

```
  ull.onmouseover=function(e){
      var x=e.target;
      if(x.nodeName.toLowerCase()==="li"){
        x.style.background="red";
      }
    };
    ull.onmouseout=function(e){
      var x=e.target;
      if(x.nodeName.toLowerCase()==="li"){
        x.style.background="#fff";
      }
    };
    btn.addEventListener("click",function(){
      var Li = document.createElement("li");
      ull.appendChild(Li);
    });
```
感觉出事件委托的作用了吧，添加事件委托，就可以不用一个个的去遍历元素的子节点，只需要给父元素添加事件，因为点击子节点可以冒泡到父元素上去。

总结来说，事件委托是通过事件冒泡的原理去减少DOM的操作，先获取当前事件的DOM操作，再获取当前标签名，对其进行操作

### 参照链接

https://www.cnblogs.com/liugang-vip/p/5616484.html
