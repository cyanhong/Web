CSS布局
---
 ### 常用方式
 * 定位布局
 * 流式布局
 * 浮动布局
 * 弹性布局
 
盒模型
---
 
 ```分为标准盒模型和IE盒模型```
 
 标准盒模型
 
 ![css1](https://github.com/cyanhong/web/blob/master/images/css-1.png)
 
 IE盒模型
 
 ![css1](https://github.com/cyanhong/web/blob/master/images/css-2.png)
 
  标准和模型中，width和height只是content的宽高
  
  IE盒模型：width和height = content+padding+border
  
  ### 怎么设置两种模型
  
  ```
   /* 标准模型 */
  box-sizing:content-box;

   /*IE模型*/
  box-sizing:border-box;
```

css设置动画效果
---

三个属性：transition（转换）、transform（变形）、animation（动画）

### transition和animation的区别

两者大部分的属性都相同，都是随时间改变元素的属性值

* ```transition``` 需要触发一个事件才能改变属性，而``` animation``` 不需要触发任何事件的情况下才会随时间改变属性值
* ```transition ```为 2 帧，从 from .... to，而 ```animation ```可以一帧一帧的。
* ```transition ```关注的是 CSS属性 的变化， 而```animation``` 作用于元素本身而不是样式属性
* ``` animation``` 制作动画必须用关键帧@keyframes声明一个动画，然后在 animation中调用关键帧声明的动画。

什么是关键帧？

@keyframes就是关键帧，而且需要加webkit前缀，比如 ：

```
/* 当鼠标悬浮在button class为login的按钮时，触发changeColor动画 */

button.login:hover {
  -webkit-animation: 1s changeColor;    // chrome和Safari
  animation: 1s changeColor;
}

@-webkit-keyframes changeColor {
  0% {
    background: #c00;
  }
  50% {
    background: orange;
  }
  100% {
    background: yellowgreen;
  }
}
@keyframes changeColor {
  0% {
    background: #c00;
  }
  50% {
    background: orange;
  }
  100% {
    background: yellowgreen;
  }
}

/* 上面代码中的0% 100%的百分号都不能省略，0%可以由from代替，100%可以由to代替。 */
```
知乎上有个答案这么说的 :
```
Transition 强调过渡，Transition ＋ Transform ＝ 两个关键帧的Animation
Animation 强调流程与控制，Duration ＋ TransformLib ＋ Control ＝ 多个关键帧的Animation
```
甚至于，我们可以说 : transition 是 Animation 的一个子集，即一个 Animation 是由多个 transition 组合而成的。

Flex布局
---

```Flex布局```，即弹性布局，用来为盒状模型提供最大的灵活性,采用Flex布局的元素，称为Flex容器，容器默认存在两根轴：水平的主轴和垂直的交叉轴

最常用来实现垂直居中

### 指定Flex布局
```
  display: flex;
```

### 六大容器属性

* ```flex-direction```:决定主轴的方向
* ```flex-wrap```：决定如果一条轴线排不下，该如何换行
* ```flex-flow```：是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
* ```justify-content```：定义项目在主轴上的对齐方式
* ```align-items```：定义项目在交叉轴上如何对齐
* ```align-content```：定义多根轴线的对齐方式，如果项目只有一根轴线，则该属性不起作用

怎么实现垂直居中
---

1 Flex布局

```
/* css */
.box{
	height: 300px;     //这里设置高度，才看的出垂直居中的效果
	display: flex;
	justify-content: center;    //主轴上的对齐方式
	align-items: center;        //交叉轴上的对齐方式
}

/* html */
<div class="box">
	<div class="child">垂直居中</div>
</div>
```
2 绝对定位+负边距

```
.box{
    	position: relative;
    	height: 200px;
    }
.child {
	position: absolute;
	left: 50%;
	top: 50%;
	width: 200px;
	height: 50px;
	margin-left: -100px;   //子元素宽度的一半
	margin-top: -25px;     //子元素高度的一半
}
```
3 利用table-cell（未脱离文本流）

```
.box{
	display: table;   //变成块级表格元素
	width:800px;
	height: 500px;
	text-align: center;   //水平居中
}
.child {
	display: table-cell;  // 变成表格单元格
	background: red;
	vertical-align: middle;  // 垂直居中
}
```
4 -webkit-box

```
.box{
  	display: -webkit-box;
	width: 800px;
   	height: 500px;
    	-webkit-box-orient: horizontal;  // 父容器里子容器的排列方式，是水平还是垂直
    	-webkit-box-pack: center;        // 父容器里子容器的水平对齐方式
    	-webkit-box-align: center;       // 父容器里子容器的垂直对齐方式
}
```
5 绝对定位+ transfom （未知宽高情况下使用）

```
.box{
	position: relative;
	height: 200px;
}
.child {
	position: absolute;
	left: 50%;
	top: 50%;
	transform: translate(-50%,-50%);
}
```

6 绝对定位 + 0 （已知高度和宽度）

```
.box{
	position:relative;
	width:1000px;
	height:500px;
	background:#ccc;
}
.child{
	position:absolute;
	top:0;
	left:0;
	bottom:0;
	right:0;
	width:100px;   //宽度
	height:100px;  //高度
	overflow:auto;
	margin:auto;
	background:red;
}
```
### 总结

#### 通用方式

* Flex布局
* 绝对定位+负边距
* table-cell
* -webkit-box

#### 不知道宽和高

绝对定位+transform

#### 已知宽度和高度

绝对定位 + 0

CSS3 新特性
---

面试中，最好说下自己用过的一些新特性，而不是拿自己死记硬背的东西

```transition``` 过渡
* transition-property: width;   对象参与过渡的属性
* transition-duration: 1s;      过渡的持续时间
* transition-timing-function: linear;  过渡的类型
* transition-delay: 2s;         延迟过渡的时间

```
transition: all 2s ease-in 0s;
	 //  分别对应上述四个属性
	 // ease-in:加速  ease-out：减速 linear：匀速
```

```transform``` 变形转换
主要包括translate（水平移动）、rotate（旋转）、scale（旋转）、scale（伸缩）、skew（倾斜）

```animation``` 动画

* animation-name 规定需要绑定到选择器的 keyframe
* animation-duration 规定完成动画所花费的时间，以秒或毫秒计
* animation-timing-function 规定动画的速度曲线
* animation-delay 规定在动画开始之前的延迟
* animation-iteration-count 规定动画应该播放的次数
* animation-direction 规定是否应该轮流反向播放动画
```
div
{
	width:100px;
	height:100px;
	background:red;
	position:relative;
	animation:mymove 5s infinite;
	-webkit-animation:mymove 5s infinite; /*Safari and Chrome*/
}

@keyframes mymove
{
	from {left:0px;}
	to {left:200px;}
}

@-webkit-keyframes mymove /*Safari and Chrome*/
{
	from {left:0px;}
	to {left:200px;}
}
```
```border-radius```圆角
```
border-radius: 5px;
```
```box-shadow``` y阴影

```
div{
	box-shadow: 10px 10px 5px #ccc;
}
```
```-webkit-box```水平垂直居中

```
.box{
  	display: -webkit-box;
	width: 800px;
   	height: 500px;
    	-webkit-box-orient: horizontal;  // 父容器里子容器的排列方式，是水平还是垂直
    	-webkit-box-pack: center;        // 父容器里子容器的水平对齐方式
    	-webkit-box-align: center;       // 父容器里子容器的垂直对齐方式
}
```








 
 
 
