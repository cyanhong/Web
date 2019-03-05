要想解释清楚什么是闭包，得先弄清楚作用域链

什么是作用域链
---
作用域链，顾名思义就是由作用域链串起来的一条链

当代码在一个环境中执行时，会创建变量对象的一个作用域链，，它的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。
总结来说，就是保证能有有序访问各个变量和函数

作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象。活动对
象在最开始时只包含一个变量，即 arguments 对象（这个对象在全局环境中是不存在的）。作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。

是不是有点懵？

来看代码

```
  var color = "blue"; 
  function changeColor(){ 
     var anotherColor = "red"; 
     function swapColors(){ 
       var tempColor = anotherColor; 
       anotherColor = color; 
       color = tempColor; 

   // 这里可以访问 color、anotherColor 和 tempColor 
     } 
   // 这里可以访问 color 和 anotherColor，但不能访问 tempColor 
    swapColors(); 
  } 
  // 这里只能访问 color 
  changeColor(); 
```
上面这段代码与3个执行环境：全局环境、changeColor()的局部环境和swapColors()的局部环境，从上往下执行程序，首先是全局环境放在作用域链上的最末端，然后是changeColor局部环境放在全局环境的后面，再这个局部环境中又有一个swapColors的局部环境，于是紧跟着changeColor的后面

现在作用域链长这个样子 （最末端）全局环境→→→→→changeColor→→→→→swapColors（最前端）

changeColor局部环境可以访问全局环境中的color变量，但不能访问swapColors中的变量，swapColors局部环境可以访问changeColor中的anotherColor变量，也可以访问全局环境的变量。
