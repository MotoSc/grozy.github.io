title: "AutoLayout & VFL"
date: 2014-11-26 11:10:36
tags:
---
自iOS6之后的版本都可以使用AutoLayout
##一、AutoLayout的使用场景
1、苹果机型逐步增多，为了适配更多的机型，应用应该使用AL。
2、根据应用内容决定。如果内容的信息众多，同时需要展示的类别也很多，尺寸动态不固定。
3、Mac os中窗口大小的变化。
4、支持横屏旋转的iPad应用。（iPhone支持横屏的应该很少吧）
5、业务不负责，页面较少的应用
##二、AutoLayout的基础理论
###1、AL的核心出发点
（1）view具有自我计算尺寸，布局的能力。通过自身的内容，能够获得尺寸。也就是有intrinsic content size的控件（例如button，label）。通俗的讲，具有intrinsic content size的控件自己知道（可以计算）自己的大小，例如一个label，当你设置text，font之后，其大小是可以计算到的。
（2）view的布局位置，确定它与superview及其它view的关系。
（3）与传统的autoresizingmask自适应相比，AL更加精确，精确view的布局位置。
（4）view不一定需要一个初始化的Rect。AL中，View如果有足够的constraint，便能够确定自己的尺寸和位置，并且知道自己和其它view的关系。也就是说，如果想确定view的布局，就该给它（们）添加constraint。
##三、xib和SB下的AutoLayout
开启xib或者SB之后，选择view(s)。Editor->Pin，Pin子菜单的项目就是可用的constraint
1、Width:固定自身宽度
2、Height:固定自身高度
3、Horizontal-Spacing:固定两个view的水平间距
4、Vertical-Spacing:固定两个view的垂直间距
5、...Space to Superview:这四个相同后缀的是view相对于左、右、上、下的间距。
6、Widths Equally:两个view保持宽度相同
7、Height Equally:两个view保持相同高度
在xib和SB界面中右下角的工具条也可以编辑constraint
每个constraint添加后都是可以编辑的。选中某个constraint后开启右边栏选inspector。可以修改数值（这个数值是通过view之间的偏移量）和优先级。
xib和SB的比较直观，可以直接看到效果和误差的值，如果有不正确的constraint时，会有提示和补全。
##四、编码方式使用AutoLayout
###1、VFL
VFL在程序中可以通过
``` bash
+ (NSArray *)constraintsWithVisualFormat:(NSString *)format options:(NSLayoutFormatOptions)opts metrics:(NSDictionary *)metrics views:(NSDictionary *)views;  
```