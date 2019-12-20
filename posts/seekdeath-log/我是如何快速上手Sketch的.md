# 我是如何快学上手 Sketch 的

地址：https://shevakuilin.com/learn-sketch/

创建时间：2019-11-15

## 简介

如何在短时间内快速学习并掌握 Sketch  常用的基础操作，本文记录了我从 0 基础开始的学习过程与快速上手的使用技巧总结，希望能够对你有所帮助

## 引言

这是一篇快速入门指南，究竟有多快速呢，比如我，在没有任何设计软件使用经验的前提下，从 Sketch 安装完成开始，算上练习时间总共也就 `1小时`。

所以，如果你希望在很短的时间内掌握 Sketch 的常用基础操作，并能够有所产出的话，那么看这篇文章就足够了。

## 我为什么要写这篇文章

使用 Sketch 的契机，源于在开发我的个人应用 Each 期间，缺少 UI 设计资源，迫切的需要使用一款设计软件来进行图形绘制工作。在过往的工作中与设计师协作时，最常用的组合就是 Sketch + Zepin，所以对 Sketch 的印象非常好，选择了这款应用作为 Each 的设计产出工具。

倘若是自认为理解能力比较强的同学，其实完全可以参照这个 [Sketch入门实战教程](https://www.jianshu.com/c/d7c58b8cc750) 来练习，这是一个已断更的系列教程，总共 6 篇，基本上可以说是手把手教你了，算是我的 Sketch 学习路上的启蒙读物。

但是几篇文章有两个缺点：

1. Sketch 版本比较老，如果你是新安装的 Sketch，很多图形界面或操作控件的位置都发生了变化，找起来很费时。
2. 很多关键操作点，作者没有叙述清楚，或者干脆就忽略了，我是通过对文中的 GIF 图片**逐帧分解**，才找到其中法门，对于一个纯新手来讲，还是很坑的。

基于以上前提，才有了本文的诞生，上面的所有坑都是我一一踩过的，所以阅读本文你不用担心遇到这些问题。

## 阅读完本文你将获得什么

你将收获：

1. 熟练使用 Sketch 常用操作快捷键
2. 熟悉 Sketch 常用操作界面 & 控件的作用
3. 熟悉 APP 切图的绘制方法
4. 能够独立设计一个炫酷的 Logo

## 环境

本文的操作环境如下：

- 电脑系统：macOS Catalina 10.15.1 Beta版
- Sketch 版本: 59.1

## 通过练习熟悉操作

话不多说，我们直接开始上手练习，在练习中去逐渐熟悉各个操作，并且当你每完成一个练习，都会得到正向的反馈，会很有成就感。

### 练习1 - 爱心

涉及知识点：

**正方形**、**圆形**、**编组**

先从最简单的开始练习，画一个红色的爱心。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E7%88%B1%E5%BF%83%E6%95%88%E6%9E%9C%E5%9B%BE.jpg)

首先打开 Sketch，选择`新文档`

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E5%88%9B%E5%BB%BA%E6%96%B0%E6%96%87%E6%A1%A3.jpg)

**第一步，我们建立一个画板**，按快捷键`A`，大小设置成 `500*500`，画板默认为白色。然后在画板上，画一个大小为 `240*240` 的正方形，按快捷键 `R`（这个表示要画一个矩形）, 然后按住 `Shift` 键（表示长宽等比 1：1，也就是正方形）, 然后将颜色填充成红色，勾掉边框颜色，如下图：

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E7%88%B1%E5%BF%83%E6%AD%A5%E9%AA%A41.jpg)

**第二步，在这个正方形之上**，画一个 `240*240` 的圆形，按快捷键 `O`，`O` 默认是椭圆， 然后按住 `Shift` 键（表示要画一个圆形），填充成红色，勾掉边框颜色，并将圆心放在正方形最上面一条边上。如下图:

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E7%88%B1%E5%BF%83%E6%AD%A5%E9%AA%A42.jpg)

**第三步，复制刚才那个圆形**，沿着正方形的右边线放置，如下图:

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E7%88%B1%E5%BF%83%E6%AD%A5%E9%AA%A43.jpg)

**第四步，将所有的元件进行编组**，旋转`-45°`就可以了。

首先，选中所有元件（一个正方形，两个圆形），然后点击 Sketch 界面上方导航栏上的 `编组`，选中左侧导航栏的编组，在最右边`x, y`的右侧 `°` 中填入 `-45`:

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E7%88%B1%E5%BF%83%E6%AD%A5%E9%AA%A44.jpg)

### 练习2 - Music 图标

主要知识点：

**椭圆**、**长方形**、**Transform 的用法**

现在已经有一些基础了，开始第二个练习，画一个 Music 图标。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%9F%B3%E7%AC%A6%E6%95%88%E6%9E%9C%E5%9B%BE.jpg)

**第一步，建立一个大小为 `400\*400` 的画板**，按快捷键 `A`

**第二步，画一个大小为 `60\*45` 的椭圆**，按快捷键 `O`，然后再画一个大小为 `23*165` 的长方形，按快捷键 `R`，通过上下左右键，移动长方形到合适的位置，如果觉得没有那么无缝链接的感觉，可以放大画板再移动，按 `Control` + `+` 放大，`-` 缩小。把椭圆和长方形组成一个编组。**有一个技巧很重要，如果要编辑组内的某一个元素，需要你按住Command键，然后鼠标点击那个元素。**

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%9F%B3%E7%AC%A6%E6%AD%A5%E9%AA%A41.jpg)

**第三步，复制刚才的那个编组，移动到合适的位置，再画一个合适的长方形**，为了好看，你可以在右边设置一下这个长方形的半径（圆角）为`10`，注意，这次我们要用到`旋转圆角`，先选中元件，然后点击 Sketch 界面顶部导航栏上的 `旋转`，此时会出现一个包含十字的圆形图标在你选中的元件上，鼠标移动到圆形图标上方，鼠标会变成一个旋转箭头图标，然后按住右边的点向上移动。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%9F%B3%E7%AC%A6%E6%AD%A5%E9%AA%A42.jpg)

旋转完成之后，就完成了一个 Music 图标的制作。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%9F%B3%E7%AC%A6%E6%AD%A5%E9%AA%A43.jpg)

### 练习3 - 铃声图标

主要知识点：

**锚点**、**网格**、**减去顶层**。

现在难度上升，在这个练习中，我们会画一个铃铛图标：

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%95%88%E6%9E%9C%E5%9B%BE.jpg)

**第一步，我们创建一个`300\*300`的画板**，然后画一个`80*80`的圆，然后再画一个`80*80`的正方形，如图所示:

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A41.jpg)

**第二步，我们来进行锚点操作**，首先，我们选中正方形，然后点击 Sketch 界面顶部导航栏的 `编辑`，在正方形靠近底部的位置左右两边分别选2个点，在这里我们需要借助于`网格`来操作，可以在Sketch 的顶部工具栏（电脑屏幕最上方的工具栏）里选择`视图`->`画布`->`显示网格`，也可以用快捷键 `Control+G`，分别对最下面的2个点进行拉伸，拉伸的位置需要左右对其，这个时候，网格就会帮上很大的忙了，很轻松的我们就可以左右对称了。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A42.jpg)

**第三步，我们选中中间的2个锚点**，使其圆角等于`30`，这样就显得很圆滑了。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A43.jpg)

同样的，我们对最底部的2个锚点，使其圆角等于`2`。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A44.jpg)

**第四步，最底部小半圆的生成**，我们先把上面画的部分进行`编组`，往上面整体拖动，方便下面小半圆的绘制。
画一个`30*30`的圆形，然后再圆形的上面画一个`30*60`的长方形。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A45.jpg)

同时选中这2个，然后进行`减去顶层`操作，最后在把合成的小半圆移动到一个合适的位置

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A46.jpg)

**第五步，铃铛顶部**，画一个大小为`25*10`的矩形，圆角设置为`5`，放到铃铛顶部，露出一半边就可以了

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E9%93%83%E9%93%9B%E6%AD%A5%E9%AA%A47.jpg)

### 练习4 - 扬声器图标

主要知识点：

**钢笔锚点**、**网格校对**、**剪刀**。

下面来画一个扬声器图标:

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%95%88%E6%9E%9C%E5%9B%BE.jpg)

**第一步:我们建立一个画板**，大小`500*500`，然后画2个空心圆（空心圆只要边框，不要填充），大小分别为`160*160`，`80*80`，并用`网格`进行校对，如下图：

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A41.jpg)

**第二步，建立一个和大圆相同大小的长方形**，遮住大圆的左边（可以不用对准中线，稍微向左挪动一两个像素，完全对准可能出现后面步骤无法用剪刀裁剪路径的问题），同时选中长方形和大圆，进行`减去顶层`，得到如下的图形：

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A42.jpg)![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A43.jpg)![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A44.jpg)

第四步：对上面2个路径用`剪刀`裁剪，选中元件，然后屏幕顶部工具栏选择`图层` -> `路径` -> `剪刀`，把多余边沿虚线给剪切掉。

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A46.jpg)![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A47.jpg)

然后在左边放2个长方形，高度和里面的小圆相同大小80，然后对中间的长方形进行`锚点拉伸`操作，记得要借助于网格：

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/%E6%89%AC%E5%A3%B0%E5%99%A8%E6%AD%A5%E9%AA%A49.jpg)

最后再排放到合适的位置上，扬声器就完成了。

## 设计一个 Each Logo

好了，现在我们开始尝试设计一个炫酷的 Logo，就像下面这两张一样：

<img src="https://github.com/shevakuilin/GhostImagesSketch/raw/master/鹅毛笔logo.jpg" width="353" height ="355" />



<img src="https://github.com/shevakuilin/GhostImagesSketch/raw/master/EachLogo.png" width="353" height ="355" />

我们以第二张 Each Logo 为例，来教大家如何设计一个 Logo

主要知识点：

**交集与差集的结合运用、梯度渐变色**。

**第一步，创建一个 400 * 400 的画板，快键键 U 创建 280 * 45 的圆角矩形**，使其居中对齐在画板当中，然后创建两个 `200 * 45` 的圆角矩形，使其水平居中第一个圆角矩形，间距 `18`，再创建两个 `160 * 45` 的圆角矩形，分别水平对齐第一个圆角矩形的左右两个边线，间距与第二次创建的两个矩形之间也是 `18`

![img](https://github.com/shevakuilin/GhostImagesSketch/raw/master/Logo设计步骤1.jpg)