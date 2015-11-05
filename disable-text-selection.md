# [译]用CSS来禁止文本选择

> **原文：**[Disable Text Selection with CSS](http://sixrevisions.com/css/disable-text-selection/)


有些时候我们需要禁止网页的部分文本不能被选择。你可以使用`user-select`这个CSS特性来实现这个需求。

## 举例

下面是一个使用了`disable-selection`类的样式规则，当它作用于一个HTML元素的时候，它会让我们不能够选择这个元素。

	.disable-selection{
		-moz-user-select:none; /* Firefox */
		 -ms-user-select:none; /* Internet Explorer */
	   -ktml-user-select:none; /* KHTML浏览器（比如：Konqueror） */
     -webkit-user-select:none; /* Chrome,Safari,and Opera */
   	-webkit-touch-callout:none; /* Disable Android and IOS callout */
	}

关于这些样式的一些细节的解释：

- `-webkit-user-select`是给Chrome,Safari和Opera用的（并不需要使用`-o-user-select`）。
- 没有前缀的`user-select`是被故意略去的。
- `-webkit-touch-callout`属性可以让在移动设备上的触摸后弹出失效。就像下面的这些，我们可以让它们不能出现。

![disable-callout](imgs\disable-text-selection\disable-callout.jpg)

## 演示

![disable-selection](imgs\disable-text-selection\disable-text-selection.jpg)

[查看演示](http://cdn.sixrevisions.com/demos/disable-text-selection/index.html)

## 要时常记住的

有一个陷阱就是：`user-select`并不是W3C规范中标准的CSS特性。尽管`user-select`通过添加浏览器前缀有很好的浏览器支持。

前面的例子中，我没有使用没有前缀的`user-select`特性声明。那是因为在web标准中根本就没有这个属性。我们可以对它的使用类比于于IE专有的CSS属性`ms-filter`或者`-ms-text-kashida-space`的属性的使用。

其他需要注意的地方：

- `user-select`是**有问题并且是不稳定的**。有些时候你依然还是可以选择文本，特别是当你从没有被屏蔽掉文本选择的文本的那部分开始选择。
- **使用全选快捷键有些时候还是会把屏蔽文本选择**(Win:Ctrl+A/Mac:Cmd+A)。这种情况你可以在IE11中清楚的了解到。
- **这不是一种万全的保证文本不被选择的策略。**CSS能够很容易被屏蔽。这种技巧依赖于非标准的CSS特性，这意味着未来对这个属性的可持续支持上面存在着很大的不确定性。
- **屏蔽掉文本选择是很恼人的。**我会在渐进提升的过程中使用这个技巧：只有当它可以提高使用支持这个`user-select`特性的浏览器和设备的用户的用户体验的时候才使用它。但是，我绝不会把它设置成一个大范围的CSS选择器像全部选择器(*）或者`body`.
- **这个`user-select`特性可能会让你的样式表失效**。如果遵循标准对你来说非常重要，使用这个属性会在你使用规范测试比如说[CSS Validation Service](https://jigsaw.w3.org/css-validator/)的使用出现问题

![user-select-validation](imgs\disable-text-selection\user-select-validation.png)

## 浏览器支持

**更新于：**2015年3月

|浏览器|版本支持（以上）|
|:----|:---|
|Chrome|6|
|Firefox|2|
|IE|10|
|Safari|3.1|

移动端
|浏览器|版本支持（以上）|
|:------|:-----|
|Chrome(Android)|2.1|
|Safari(IOS)|3.2|


