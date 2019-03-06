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
* ``` animation``` 制作动画必须用关键帧keyframes声明一个动画，然后在 animation中调用关键帧声明的动画。












 
 
 
