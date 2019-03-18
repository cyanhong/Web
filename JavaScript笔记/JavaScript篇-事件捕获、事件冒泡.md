###先来看看基本概念
```
事件捕获：从window一直到当前点击的元素，即从上往下
事件冒泡：顾名思义，往上冒泡，所以是从当前点击的元素一直到window
```

通过今天的面试，总结出光了解概念是不行的，还是得自己动手写代码

```
  <div id="obj1">我是第一
      <div id="obj2">我是第二
        <p id="obj3">我是第三</p>
      </div>
   </div>
   
   //事件捕获
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
//点击我是第三，先弹出 我是obj1，再弹出 我是obj2，再弹出我是obj3
```
```
 //事件冒泡
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
//点击我是第三，先弹出 我是obj3，再弹出 我是obj2，再弹出我是obj1
```
