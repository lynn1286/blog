---
title: css面试题
date: 2020-12-07 10:09:45
categories: 面试题
tags: css
---

## 谈谈你对 CSS 盒模型的认识

  盒子模型分两种:

    1. 标准模型
    2. IE模型

  标准模型: 标准 W3C 盒子模型的范围包括 margin、border、padding、content，并且 content 部分不包含其他部分。
  IE模型: IE 盒子模型的范围也包括 margin、border、padding、content，和标准 W3C 盒子模型不同的是：IE 盒子模型的 content 部分包含了 border 和 pading。

## 什么是BFC

  基本概念： Block Formatting Context, 块级格式化上下文，一个独立的块级渲染区域，该区域拥有一套渲染规格来约束块级盒子的布局，且与区域外部无关。

  BFC 的原理：

    1. BFC 这个元素的垂直的边距会发生重叠
    2. BFC 的区域不会与浮动元素的 float 重叠
    3. 独立的容器，内外元素互不影响
    4. 计算 BFC 高度，浮动元素也参与计算

  如何创建 BFC：

    1. float 不为none的时候
    2. position 不为 static 或者 relative 的时候
    3. display 与 table 相关的时候
    4. overflow 为auto, hidden 的时候

## px、em、rem、%、vw、vh、vm这些单位的区别

  1. px就是pixel的缩写，意为像素。px就是一张图片最小的一个点，一张位图就是千千万万的这样的点构成的，比如常常听到的电脑像素是1024x768的，表示的是水平方向是1024个像素点，垂直方向是768个像素点。
  2. em参考物是父元素的font-size，具有继承的特点。如果自身定义了font-size按自身来计算（浏览器默认字体是16px），整个页面内1em不是一个固定的值。
  3. rem 是css3新单位，相对于根元素html（网页）的font-size，不会像em那样，依赖于父元素的字体大小，而造成混乱。
  4. % 一般宽泛的讲是相对于父元素，但是并不是十分准确。对于普通定位元素就是我们理解的父元素 ； 对于position: absolute;的元素是相对于已定位的父元素 ； 对于position: fixed;的元素是相对于 ViewPort（可视窗口）
  5. vw 是 css3新单位，viewpoint width的缩写，视窗宽度，1vw等于视窗宽度的1%。 举个例子：浏览器宽度1200px, 1 vw = 1200px/100 = 12 px。
  6. vh 是 css3新单位，viewpoint height的缩写，视窗高度，1vh等于视窗高度的1%。 举个例子：浏览器高度900px, 1 vh = 900px/100 = 9 px。
  7. vm 是 css3新单位，相对于视口的宽度或高度中较小的那个。其中最小的那个被均分为100单位的vm。 举个例子：浏览器高度900px，宽度1200px，取最小的浏览器高度， 1 vm = 900px/100 = 9 px。

## css3有哪些新特性

  圆角（border-radius）
  阴影（box-shadow）
  过渡效果（transition）
  翻转（transform）
  动画（animation）
  媒体查询（@media）
  弹性盒子（flex）

## 垂直居中有哪些方法

  1. 使用绝对定位和负外边距对块级元素进行垂直居中

      ```html
        <div id="box">
          <div id="child"></div>
        </div>
      ```

      ```css
      #box {
        width: 300px;
        height: 300px;
        background: #ddd;
        position: relative;
      }
      #child {
        width: 150px;
        height: 100px;
        background: orange;
        position: absolute;
        top: 50%;
        margin: -50px 0 0 0; 
      }
      ```

  2. 使用绝对定位和transform

      ```html
        <div id="box">
            <div id="child">test vertical align</div>
        </div>
      ```

      ```css
        #box {
            width: 300px;
            height: 300px;
            background: #ddd;
            position: relative;
        }
        #child {
            background: orange;
            position: absolute;
            top: 50%;
            transform: translate(0, -50%);
        }
      ```

  3. 使用绝对定位和负外边距进行垂直居中的方式

      ```html
        <div id="box">
            <div id="child">test vertical align</div>
        </div>
      ```

      ```css
        #box {
            width: 300px;
            height: 300px;
            background: #ddd;
            position: relative;
        }
        #child {
            width: 50%;
            height: 30%;
            background: orange;
            position: absolute;
            top: 50%;
            margin: -15% 0 0 0;
        }
      ```

  4. 绝对定位结合margin: auto

      ```html
        <div id="box">
            <div id="child">test vertical align</div>
        </div>
      ```

      ```css
        #box {
            width: 300px;
            height: 300px;
            background: #ddd;
            position: relative;
        }
        #child {
            width: 200px;
            height: 100px;
            background: orange;
            position: absolute;
            top: 0;
            bottom: 0;
            margin: auto;
            line-height: 100px;
        }
      ```

  5. 使用padding实现子元素的垂直居中

      ```html
        <div id="box">
            <div id="child"></div>
        </div>
      ```

      ```css
        #box {
            width: 300px;
            background: #ddd;
            padding: 100px 0;
        }
        #child {
            width: 200px;
            height: 100px;
            background: orange;
        }
      ```

  6. 设置第三方基准

      ```html
        <div id="box">
            <div id="base"></div>
            <div id="child"></div>
        </div>
      ```

      ```css
        #box {
            width: 300px;
            height: 300px;
            background: #ddd;
        }
        #base {
            height: 50%;
            background: orange;
        }
        #child {
            height: 100px;
            background: rgba(131, 224, 245, 0.6); 
            margin-top: -50px;
        }
      ```

  7. 使用flex布局

      ```html
        <div id="box">test vertical align</div>
      ```

      ```css
      #box {
          width: 300px;
          height: 300px;
          background: #ddd;
          display: flex;
          align-items: center;
      }
      ```

  8. 使用 line-height 对单行文本进行垂直居中

      ```html
        <div id="box">test vertical align</div>
      ```

      ```css
      #box{
          width: 300px;
          height: 300px;
          background: #ddd;
          line-height: 300px;
      }
      ```

  9. 使用 line-height 和 vertical-align 对图片进行垂直居中

      ```html
        <div id="box">
            <img src="xx.jpg">
        </div>
      ```

      ```css
      #box{
          width: 300px;
          height: 300px;
          background: #ddd;
          line-height: 300px;
      }
      #box img {
          width: 200px;
          height: 200px;
          vertical-align: middle;
      }
      ```

  10. 使用 display: table; 和 vertical-align: middle; 对容器里的文字进行垂直居中

      ```html
        <div id="box">
            <div id="child">test vertical align</div>
        </div>
      ```

      ```css
      #box {
        width: 300px;
        height: 300px;
        background: #ddd;
        display: table;
      }
      #child {
          display: table-cell;
          vertical-align: middle;
      }
      ```

  11. 使用 CSS Grid

      ```html
        <div id="box">
            <div class="one"></div>
            <div class="two">target item</div>
            <div class="three"></div>
        </div>
      ```

      ```css
      #box {
        width: 300px;
        height: 300px;
        display: grid;
      }
      .two {
          background: orange;
      }
      .one, .three {
          background: skyblue;
      }
      ```

  总结一下，比较通用的就三种：
  flex布局
  绝对定位 + translate，或绝对定位 + margin
  grid布局

## css精灵图怎么实现，好处和坏处

  CSS Sprites优点:
    1. 利用CSS Sprites能很好地减少了网页的http请求，从而大大的提高了页面的性能，这也是CSS Sprites最大的优点，也是其被广泛传播和应用的主要原因；
    2. CSS Sprites能减少图片的字节，曾经比较过多次3张图片合并成1张图片的字节总是小于这3张图片的字节总和。

  CSS Sprites缺点:
    1. 在图片合并的时候，你要把多张图片有序的合理的合并成一张图片，还要留好足够的空间，防止板块内不会出现不必要的背景；这些还好，最痛苦的是在宽屏，高分辨率的屏幕下的自适应页面，你的图片如果不够宽，很容易出现背景断裂；
    2. CSS Sprites在开发的时候比较麻烦，你要通过photoshop或其他工具测量计算每一个背景单元的精确位置，这是针线活，没什么难度，但是很繁琐；幸好腾讯的鬼哥用RIA开发了一个CSS Sprites 样式生成工具，虽然还有一些使用上的不灵活，但是已经比photoshop测量来的方便多了，而且样式直接生成，复制，拷贝就OK！
    3. CSS Sprites在维护的时候比较麻烦，如果页面背景有少许改动，一般就要改这张合并的图片，无需改的地方最好不要动，这样避免改动更多的css，如果在原来的地方放不下，又只能（最好）往下加图片，这样图片的字节就增加了，还要改动css。
