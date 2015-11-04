# 深入了解CSS Box Shadow

> **原文：**[A Close Look at CSS Box Shadow](http://sixrevisions.com/css/css-box-shadow/)

CSS的`box-shadow`可以被用来给块级元素一个外阴影或者是内阴影。接下来让我们仔细地看一下这个CSS的特性吧。

## 举例

下面有三个把CSS的`box-shadow`属性使用在`div`里的例子。

###　例１：简单的外阴影

下面是是给副标题添加阴影的样式。

	box-shadow:0 0 10px gray;

![simple-drop-shadow](imgs\css-box-shadow\basic-box-shadow-example.png)

### 例2：内阴影

一个内阴影可以通过使用inset属性值来渲染出来。

	box-shadow:inset 0 0 10px;

![inner-shadow](imgs\css-box-shadow\basic-inset-box-shadow-example.png)

### 例3：偏移外阴影

这个例子中我们通过水平和垂直方向下和右偏移5px来实现阴影向右下方偏移

	box-shadow:5px 5px 10px;

![offset-drop-shadow-bottom-right](imgs\css-box-shadow\offset-box-shadow-example-01.png)

加入你想要阴影向左上方偏移呢？我们可以通过将水平和垂直方向的偏移量设置为-5px，正如下面的例子：

	box-shadow:-5px -5px 10px;

![offset-drop-shadow-top-left](imgs\css-box-shadow\offset-box-shadow-example-02.png)

既然你已经看到了`box-shadow`在现实中的使用，接下来让我们更加深入地了解一下。

## 语法

`box-shadow`的一般语法如下：

	box-shadow:[inset] [horizontal offset] [vertical 
	offset] [blur radius] [spread distance] [color]

## CSS属性值

CSS的`box-shadow`可能会有6个属性值：

1. inset
2. horizontal offset
3. vertical offset
4. blur radius
5. spread distance
6. color

只有两个属性是必须的：水平偏移和垂直偏移量。

四个属性值，水平偏移，垂直偏移，模糊半径，扩展距离，必须使用CSS长度单元（比如：px,em,%等）

这个颜色值必须是必须是一个颜色单元，比如十六进制值（如：#000000）。




### 属性和值总结表

|属性|是否必须|单位|默认值|
|----|----|-----|-----------|
|inset|不是|关键词|没有指定的时候，阴影默认在外面|
|水平偏移|是|长度|没有默认值，一定有指定|
|垂直偏移|是|长度|没有默认值，一定要指定|
|模糊半径|不是|长度|0|
|扩展距离|不是|长度|0|
|颜色|不是|颜色|和box shadow属性作用的元素的color值一样|

## inset

如果`inset`关键词存在，盒阴影将会放在HTML元素的内部

	box-shadow:inset 0 0 5px 5px olive;

![inset-shadow](imgs\css-box-shadow\css-box-shadow-inset-property.png)

作为对比，这里是一个没有`inset`属性的`box-shadow`样式。

	box-shadow:0 0 5px 5px olive;

![no-inset-shadow](imgs\css-box-shadow\css-box-shadow-no-inset-property.png)

### 水平偏移

水平偏移控制了盒子阴影在x轴的偏移。正值会把盒子的阴影向右移动，负值的话会把它向左移动。

下面的例子中，我们把水平的偏移设置成20px，刚好是水平偏移量的两倍，所以阴影水平宽度刚好是垂直高度的两倍。

	box-shadow:20px 10px;

![horizontal offset value](imgs\css-box-shadow\css-box-shadow-horizontal-offset-property.png)

### 垂直偏移

垂直偏移控制了盒阴影在y轴的偏移量。正值会把盒子的阴影向下移动，负值刚好相反会把盒子网上移动。

下面的例子中，垂直的偏移设置成-20px，刚好是水平偏移的两倍。同时，因为是负值，所以向上移动。

	box-shadow:10px -20px;

![vertical offset value](imgs\css-box-shadow\css-box-shadow-vertical-offset-property.png)

### 模糊半径

这个模糊半径会影响到阴影的模糊和锐利程度。

模糊半径是可选的，如果你不指定它，默认值是0.另外，你不能指定它为负值，这个和水平偏移和垂直偏移不一样。

如果模糊半径是0，盒子阴影会很锐利并且它的颜色是很实的。随着你不断的增大这个值，它会变得越来越模糊和透明。

下面的例子，模糊半径被设置成20px，因此模糊度是相当突出。

	box-shadow:5px 5px 20px;

![css-box-shadow-blur-radius](imgs\css-box-shadow\css-box-shadow-blur-radius.png)

### 扩展距离

这个扩展距离会让盒子的阴影在各个方向上都会变大或变小。如果它有一个正值，盒子阴影会在各个方向上增加大小。如果是负值，则会在各个方向上收缩。

值得注意的是，因为它的扩展距离是正5，所以会在各个方向上增加10px因为没有水平和垂直偏移。

	box-shadow:0 0 10px 5px;

![css-box-shadow-spread-distance](imgs\css-box-shadow\css-box-shadow-spread-distance.png)

当扩展距离是负的时候，阴影就会在各个方向上收缩。下面的例子展示当阴影的宽度比盒子小的时候的情况

	box-shadow:0 10px 10px -5px;

![css-box-shadow-negative-spread-distance](imgs\css-box-shadow\css-box-shadow-negative-spread-distance.png)

###　颜色

通过名字你就可以判断出来，颜色值会设置盒阴影的颜色。它可以通过任何可以表示颜色的方式来表示颜色。是否设置颜色值是可选的。

换句话说，默认情况下当你没有指明颜色值，阴影颜色会等于这个盒子对应的html元素的颜色值。比如有一个`div`的颜色被设置成红色，这个盒子阴影的颜色也会变成红色：

	color:red;
	box-shadow:0 0 10px 5px;

![css-box-shadow-default-color](imgs\css-box-shadow\css-box-shadow-default-color.png)

如果你想要设置阴影的颜色和`div`的颜色不一样，可以通过下面的方式，你会发现尽管你的文字颜色是红色，盒子阴影颜色依然可以是蓝色。

	color:red;
	box-shadow:0 0 10px 5px blue;

![css-box-shadow-color-specified](imgs\css-box-shadow\css-box-shadow-color-specified.png)

## 多阴影效果

这个就是能够让我们变得有创造力的CSS属性。你能够在一个盒子上设置多个阴影。

语法就像下面这样。

	box-shadow: [box shadow properties 1],
	 [box shadow properties 2],
	 [box shadow properties n];

换句话说，你可以通过在每个属性设置组后面添加逗号(,)来实现多阴影。

下面的例子展示了两个阴影的情况，左上方红色的阴影，右下方蓝色的阴影。

	box-shadow: -5px -5px 30px 5px red,
             5px 5px 30px 5px blue;

![multiple-box-shadows](imgs\css-box-shadow\multiple-box-shadows.png)

## 浏览器支持

这个CSS的`box-shadow`属性有着很好地浏览器支持。使用这个属性在拖后腿的IE浏览器也能在IE9以后得到支持啦。

[查看演示](http://cdn.sixrevisions.com/0457-01-css-box-shadow-demo/demo.html)

![css-box-shadow-example](imgs\css-box-shadow\css-box-shadow-examples.png)
