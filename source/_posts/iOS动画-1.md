title: iOS动画-1
date: 2014-11-19 15:55:06
tag: [动画,iOS]
---
##简诉
iOS动画，
##动画属性
###Position:
对象在屏幕中明确的X轴和Y轴坐标
###Opacity:
对象在屏幕中的透明度0.0(不可见)~0.1
###Scale:
缩放的大小依赖于初始化的大小。缩放1.0意味着将对象设置为初始化的高度和宽度。缩放0.5指的是将对象的宽度和高度都设置为初始化高度和宽度的一半。当缩放为0的时候，虽然高度和宽度都为0导致对象不可见，但是依然存在于屏幕的坐标系之中。2.0为初始化宽高的2倍。
###Color:
就和CSS动画一样，你可以将一个颜色过渡到另一个颜色。这个过度可以是文字的颜色，或者是一个形状或画板的背景色。你可以通过一次轻触，将一个颜色过渡到另一个颜色，或者展示一个新的界面给用户。
###Rotation:
对象的旋转范围为0°到360°，一个轻微旋转动画可以给一个平凡乏味的运动添加一些灵巧的元素。
###3D:
3D变换你界面中的对象，在三维中通过使用3D过度动画，使用3D旋转进入屏幕或者旋转向用户。[例子:Clear](http://realmacsoftware.com/clear)
##设计动画
如果动画是一栋房子，你可以使用工具建造房子:锤子、锯子和钉子。动画可以使用上面说过的属性，通过特定的操作去创建一个动画。所需要做的就是考虑动画和设计移动。规划动画如何发生的、动画队列的不走。在最初处于一种模糊的层次，并把动画每部分的细节写下来。然后再去规划这个动画。
对于每一个动画元素，要先想然后再去写代码。
那么问题来了？
###它要初始化的属性是什么？
你是想让它从屏幕底部离开？还是渐渐消失？或者让他变的很小？还是超大？
###它属性的终态是什么？
它是在屏幕中心？还是现在对于用户不可见？或是旋转了45°？再或者现在换了一个不同的背景色？
###这个动画需要执行多久
在实现这个动画之前，知道确切的动画时间是很困难的，所以我认为
