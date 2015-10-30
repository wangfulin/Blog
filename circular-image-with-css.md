# <译>用CSS剪切圆形图片

在这个教程，我们会介绍一下使用CSS技巧来渲染出圆形的<img>元素，主要来实现这个效果的CSS属性是border-radius.

尽管对于正方形的图片来实现圆形这个效果是相当简单的，但是对于长方形来说可能需要多一点点工作。

 ![lead](imgs\circular-image-with-css\circular-images-lead.jpg)

## 正方形图片

一个百分百正方形的img标签要变成圆形的只需要一行CSS代码。这个技巧在正方形图片上使用的最方便。

HTML
 	
	<img class="circle--square" src="woman.png" />

CSS

	.circular--squareP{
		border-radius:50%;
	}

通过设置img标签的所有的border-radius属性为正方形宽/高的50%，我们就可以把这个img标签变成圆的。

![square](imgs\circular-image-with-css\circular-image-square.png)

## 正方形图片

长方形图片会稍微有一点技巧一点。

去渲染一个圆形，必须以圆形图片为基础

要解决这个问题，我们可以通过在img标签外面套一层div，然后我们通过将超过这个外层div的img标签的内容给裁掉来实现。具体的话可以通过将外层div的overflow属性设置为hidden。

为了能够让照片的主题不要被裁掉，我们必须要区别对待水平和垂直方向的图片。

### 水平方向的图片

HTML

	<div>
		<img src="images/brack-obama.png" />
	</div>

CSS

	.circular--landscape{
		display:inline-block;
		position:relative;
		width:200px;
		height:200px;
		overflow:hidden;
		border-radius:50%;
	}
	
	.circular--landscape img{
		width:auto;
		height:100%;
		margin-left:-50%;
	}

高度和宽度属性必须要保持一样来确保这个div（.circular--landscape）能够作为正方形渲染起来

除此之外，高度和宽度属性必须要等于或者小于img的高度。这能够确保img元素能够占满外层div而不会多出一部分空白

一般来说，水平方向图片的主题（但不一定）会位于图片的中心位置。为了能够让我们尽量不会把图片的主题裁剪啦，我们可以通过把图片往左移来弥补图片剪切的内容有点偏右的问题。

我们移动img标签的大小是外层div的25%，在这个例子中就是向左50px，我们可以通过设置margin-left的属性来完成设置

	margin-left:-50px;

![landscape](imgs\circular-image-with-css\circular-image-landscape-mode.png)

图片的主题会接近图片的水平方向中心的假设并不一定是对的，最好在你选择使用这个技巧的使用把这个假设记住。

### 垂直方向的图片

HTML

	<div>
		<img src="images/woman-portrait.png" />
	</div>

CSS

	.circular--portrait{
		position:relative;
		width:200px;
		height:200px;
		overflow:hidden;
		border-radius:50%;
	}

	.circular--portrait img{
		width:100%;
		height:auto;
	}

对于垂直方向上的图片的主题在垂直方向的中心的假设当然也不适用于每一个垂直方向上的图片。

和水平方向的图片类似，外层div的宽度和高度最好等于垂直方向图片你的宽度，这样的话可以产生最好的效果。

对于垂直方向的图片，我们把宽度设置为100%，高度设置为auto（和水平方向的图片相反）

我们不需要移动这个img元素，因为这张照片的主题就在上方中心位置。

![portrait](imgs\circular-image-with-css\circular-image-portrait.png)

## 回顾

这个技巧最好适用于正方形的img标签，主题正好位于图片的中心。但是，我们的世界并不是那么完美的，所有如果需求是这样，我们就可以使用div来把长方形img标签变圆。

CSS中用来负责把图片变圆的属性是border-radius，把边的圆角变成高度/宽度的50%就可以产生一个圆。

原文链接：[circular-images-css](http://sixrevisions.com/css/circular-images-css/)
