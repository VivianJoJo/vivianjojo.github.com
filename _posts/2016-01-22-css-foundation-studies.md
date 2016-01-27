---
layout: post
title: "CSS — 基础学习笔记"
description: ""
category: CSS
tags: [CSS]
---
{% include JB/setup %}


#### CSS选择器

- 类选择器

- ID选择器

- 子选择器

- 包含(后代)选择器

- 通用选择器

- 伪类选择符

- 分组选择符

#### 盒模型--长度宽度

盒模型宽度和高度和我们平常所说的物体的宽度和高度理解是不一样的，css内定义的宽（width）和高（height），指的是填充以里的内容范围。

因此一个元素实际宽度（盒子的宽度）=左边界+左边框+左填充+内容宽度+右填充+右边框+右边界。

<img src='/include/img/CSS-盒模型-01.jpg'/>

元素的高度也是同理。比如：

css代码：

    div{
        width:200px;
        padding:20px;
        border:1px solid red;
        margin:10px;
    }


html代码：

    <body>
       <div>文本内容</div>
    </body>


元素的实际长度为：10px+1px+20px+200px+20px+1px+10px=262px。在chrome浏览器下可查看元素盒模型，如下图：

<img src='/include/img/CSS-盒模型-02.jpg'/>

#### 盒模型--填充

元素内容与边框之间是可以设置距离的，称之为“填充”。填充也可分为上、右、下、左(顺时针)。如下代码：

    div{padding:20px 10px 15px 30px;}

顺序一定不要搞混。可以分开写上面代码：

    div{
       padding-top:20px;
       padding-right:10px;
       padding-bottom:15px;
       padding-left:30px;
    }

如果上、右、下、左的填充都为10px;可以这么写

    div{padding:10px;}

如果上下填充一样为10px，左右一样为20px，可以这么写：

    div{padding:10px 20px;}

#### 盒模型 -- 边界

元素与其它元素之间的距离可以使用边界（margin）来设置。边界也是可分为上、右、下、左。如下代码：

    div{margin:20px 10px 15px 30px;}

也可以分开写：

    div{
       margin-top:20px;
       margin-right:10px;
       margin-bottom:15px;
       margin-left:30px;
    }

如果上右下左的边界都为10px;可以这么写：

    div{ margin:10px;}

如果上下边界一样为10px，左右一样为20px，可以这么写：

    div{ margin:10px 20px;}

总结一下：padding和margin的区别，`padding在边框里，margin在边框外`。

#### CSS布局模型

布局模型与盒模型一样都是 CSS 最基本、 最核心的概念。 但布局模型是建立在盒模型基础之上，又不同于我们常说的 CSS 布局样式或 CSS 布局模板。如果说布局模型是本，那么 CSS 布局模板就是末了，是外在的表现形式。

在网页中，元素有三种布局模型：

1. 流动模型 (Flow)  [demo1](http://www.imooc.com/code/2058)  [demo2](http://www.imooc.com/code/2070)

2. 浮动模型 (Float)  [demo](http://www.imooc.com/code/2071)

    块状元素这么霸道都是独占一行，如果现在我们想让两个块状元素并排显示，怎么办呢？不要着急，设置元素浮动就可以实现这一愿望。

    任何元素在默认情况下是不能浮动的，但可以用 CSS 定义为浮动，如 div、p、table、img 等元素都可以被定义为浮动。如下代码可以实现两个 div 元素一行显示。

        div{
            width:200px;
            height:200px;
            border:2px red solid;
            float:left;
        }

        <div id="div1"></div>

        <div id="div2"></div>

3. 层模型（Layer）

    * 绝对定位 [demo](http://www.imooc.com/code/2073)

        如果想为元素设置层模型中的绝对定位，需要设置`position:absolute(表示绝对定位)`，这条语句的作用将元素从文档流中拖出来，

        然后使用`left、right、top、bottom`属性相对于其最接近的一个具有定位属性的父包含块进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于浏览器窗口。

        如下面代码可以实现div元素相对于浏览器窗口向右移动100px，向下移动50px。

            div{
                width:200px;
                height:200px;
                border:2px red solid;
                position:absolute;
                left:100px;
                top:50px;
            }

            <div id="div1"></div>

    * 相对定位 [demo](http://www.imooc.com/code/2074)

        如果想为元素设置层模型中的相对定位，需要设置`position:relative（表示相对定位）`，它通过left、right、top、bottom属性确定元素在正常文档流中的偏移位置。

        相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于以前的位置移动，移动的方向和幅度由`left、right、top、bottom`属性确定，偏移前的位置保留不动。

        如下代码实现相对于以前位置向下移动50px，向右移动100px;

            #div1{
                width:200px;
                height:200px;
                border:2px red solid;
                position:relative;
                left:100px;
                top:50px;
            }

            <div id="div1"></div>

    * 固定定位 [demo](http://www.imooc.com/code/2075)

        `fixed`：表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是视图（屏幕内的网页窗口）本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终位于浏览器窗口内视图的某个位置，不会受文档流动影响，这与background-attachment:fixed;属性功能相同。以下代码可以实现相对于浏览器视图向右移动100px，向下移动50px。并且拖动滚动条时位置固定不变。

            #div1{
                width:200px;
                height:200px;
                border:2px red solid;
                position:fixed;
                left:100px;
                top:50px;
            }

            <p>文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本文本。</p>
            ....


##### Relative与Absolute组合使用

1. 参照定位的元素必须是相对定位元素的前辈元素：

        <div id="box1"><!--参照定位的元素-->
            <div id="box2">相对参照元素进行定位</div><!--相对定位元素-->
        </div>

     从上面代码可以看出box1是box2的父元素（父元素当然也是前辈元素了）。

2. 参照定位的元素必须加入`position:relative`;

        #box1{
            width:200px;
            height:200px;
            position:relative;
        }

3. 定位元素加入`position:absolute`，便可以使用top、bottom、left、right来进行偏移定位了。

        #box2{
            position:absolute;
            top:20px;
            left:30px;
        }

    这样box2就可以相对于父元素box1定位了（这里注意参照物就可以不是浏览器了，而可以自由设置了）。