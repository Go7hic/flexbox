# flexbox
flexbox 总结

![flexbox 思维导图](https://raw.githubusercontent.com/dyygtfx/flexbox/master/display%3Aflexbox%7Cinline-flexbox%20%20%20%E5%BC%B9%E6%80%A7%E7%9B%92%E5%B8%83%E5%B1%80%E5%B1%9E%E6%80%A7.png)

对应的 DEMO 为 flex.html。

## 使用 flex 布局


####弹性盒概念
弹性盒布局的定义中，它可以自动调整子元素的高和宽，来很好的填充任何显示设备中的可用显示空间，收缩内容防止内容溢出。

不同于块级元素基于垂直方向布局以及行内元素基于水平方向布局，弹性盒布局的算法是方向无关的。 虽然块级元素布局在页面中工作良好，但是其定义不足以支持那种需要根据用户代理从竖直切换成水平等变化而进行方向切换、大小调整、拉伸、收缩的引用组件。不同于将要出现的网格布局针对目标为大比例布局，弹性盒布局更适用于应用组件和小比例布局。这两种都是CSS工作组为了能与不同用户代理、不同书写模式和其他弹性需要进行协作而做出的努力。

####弹性盒布局名词
虽然弹性盒布局的讨论中自由地使用了如水平轴、内联轴和垂直轴、块级轴的词汇，仍然需要一个新的词汇表来描述这种模型。参考下面的图形来学习后面的词汇。图形显示了弹性容器有一个值为row的属性flex-direction，其意义在于包含的子元素相互之间会根据书写模式和文本流方向在主轴上水平排列，即从左到右。

![flex_terms.png](https://developer.mozilla.org/files/3739/flex_terms.png)

#####弹性容器
弹性子元素的父元素。 通过设置display 属性的值为flex 或 inline-flex将其定义为弹性容器。
#####弹性子元素
弹性容器的每一个子元素变为一个弹性子元素。弹性容器直接包含的文本变为匿名的弹性子元素。

#####轴
每个弹性盒布局以两个轴来排列。弹性子元素沿着主轴依次相互排列。侧轴垂直于主轴。

- 属性 flex-direction 定义主轴方向。
- 属性 justify-content 定义了弹性子元素如何在当前线上沿着主轴排列。
- 属性 align-items 定义了弹性子元素如何在当前线上沿着侧轴排列。
- 属性 align-self 覆盖父元素的align-items属性，定义了单独的弹性子元素如何沿着侧轴排列。
#####方向
弹性容器的主轴开始、主轴结束和侧轴开始、侧轴结束边缘代表了弹性子元素排列的起始和结束位置。它们具体取决于由writing-mode（从左到右、从右到左等等）属性建立的向量中的主轴和侧轴位置。

- 属性 order 将元素依次分组，并决定谁先出现。
- 属性 flex-flow 是属性 flex-direction 和 flex-wrap 的简写，用于排列弹性子元素。
#####行
弹性子元素根据 flex-wrap 属性控制的侧轴方向（在这个方向上可以建立垂直的新线），既可以是一行也可以是多行排列。

#####尺寸
弹性子元素宽高可相应地等价于主尺寸和侧尺寸，它们都分别取决于弹性容器的主轴和侧轴。

- min-height 和 min-width 属性的初始值为新增关键字 auto。
- 属性 flex 是 flex-basis，flex-grow 和 flex-shrink 的缩写，代表弹性子元素的伸缩性。
####建立弹性盒子
使用CSS建立弹性盒子样式，为 display 指定下面的值：

display : flex

或者

display : inline-flex

这样做将元素定义为弹性容器，其子元素即弹性子元素。 flex 值表示弹性容器为块级。inline-flex 值表示弹性容器为原子行级元素 。

>注意：浏览器厂商的前缀标记，是加在属性值而不是向属性名前面。如：display:-webkit-flex。


####弹性子元素的注意事项
包含在弹性容器内的文本自动成为匿名的弹性子元素。然而，只包含空白的弹性子元素不会被渲染，就好像它被设定为 display:none 一样。

弹性容器的绝对定位的子元素会被定位，因此其静态位置会根据它们的弹性容器的主起始内容盒的角落上开始。

目前由于一个已知的问题，在弹性子元素上指定 visibility:collapse
会导致其好像被指定了 display:none 一样，但该操作的初衷是使元素具有好像被指定了 visibility:hidden 一样的效果。在该问题被解决之前建议使用visibility:hidden ，其效果在弹性子元素上等同于 visibility:collapse 。

相邻的弹性子元素不会发生外边距合并。使用 auto 的外边距会在垂直和水平方向上带来额外的空间，这种性质可用于对齐或分隔临近的弹性子元素。W3C弹性盒子布局模型的 使用'auto'的外边距进行对齐 部分有更多信息。

为了保证弹性子元素有一个合理的默认最小尺寸，使用 min-width:auto 与 min-height:auto。对于弹性子元素， auto 属性计算其最小的宽高不小于其内容的尺寸，保证在渲染时其大小足够容纳内容。 min-width 和 min-height 部分有更多信息

弹 性盒子的对齐会进行真正的居中，不像CSS的其他居中方法。这意味着即使弹性容器溢出，子元素仍然保持居中。有些时候这可能有问题，然而即使溢出了页面的 上沿，或左边沿（在从左到右的语言如英语；这个问题在从右到左的语言中发生在右边沿，如阿拉伯文），因为你不能滚动到那里，即使那里有内容！在将来的版本 中，对齐属性会被扩展为有一个“安全”选项。目前，如果关心这个问题，你可以使用外边距来达到居中的目的，这种方式比较“安全”，在溢出的情况下会停止居 中。在你想要居中的弹性子元素上应用自动外边距代替align-属性。在弹性容器的第一个和最后一个子元素的外侧应用自动外边距来代替justify-属性。自动外边距会伸缩并假定剩余空间，当有剩余空间时居中弹性子元素，没有剩余空间时切换会正常对齐模式。然而，如果你想要在多线弹性盒子中用基于外边距的居中替换justify-content属性，你可能就没那么幸运了，因为你需要在每一线的第一个和最后一个元素上应用外边距。除非你能提前预测哪个元素是哪一线上的最后一个元素，否则你没法稳定地在主轴上用基于外边距的居中代替 justify-content 属性。

说起虽然元素的显示顺序与源代码中的顺序无关，这种无关仅对显示效果有效，不包括语音顺序和基于源代码的导航。即使是 order 也不影响语言和导航序列。因此开发者必须小心排列源代码中的元素以免破坏文档的可访问性。

####弹性盒子的属性
>Flexible-boxes-related properties: margin, align-content, align-items, align-self, display, flex, flex-basis, flex-direction, flex-flow, flex-grow, flex-shrink, flex-wrap, justify-content, min-height, min-width, order


#####不影响弹性盒子的属性

因为弹性盒子使用一种不同的布局逻辑，一些属性会在弹性容器上无效。

- 多列模块 中的column-*属性对弹性子元素无效。
- float 和 clear 对弹性子元素无效。使用 float 会导致 display 属性计算为 block.
- vertical-align 对弹性子元素的对齐无效。
####始终要记得的一些事
描述弹性子元素如何排列的逻辑有时候非常隐晦。这是一些在计划使用弹性盒子时应避免的一些事。

弹性盒子会根据书写模式的需要进行排列。这意味着主起始点和主结束点需要根据开始和结束点布局。

侧起始点和侧结束点取决于依赖 direction 属性值的开始和结束点的定义。

只要 break- 属性允许，断页可能出现在弹性盒子布局中。CSS3的 break-after， break-before 和 break-inside a以及CSS2.1的 page-break-before，page-break-after 和 page-break-inside 属性可能出现在弹性容器，弹性子元素以及弹性子元素的内部元素上出现。


