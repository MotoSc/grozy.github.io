title: "AutoLayout & VFL"
date: 2014-11-26 11:10:36
tags: [AutoLayout,VFL]
---
自iOS6之后的版本都可以使用AutoLayout[大部分内容转自](http://blog.csdn.net/mozixiong/article/details/14165391)
##AutoLayout的使用场景
1、苹果机型逐步增多，为了适配更多的机型，应用应该使用AL。
2、根据应用内容决定。如果内容的信息众多，同时需要展示的类别也很多，尺寸动态不固定。
3、Mac os中窗口大小的变化。
4、支持横屏旋转的iPad应用。（iPhone支持横屏的应该很少吧）
5、业务不负责，页面较少的应用
##AutoLayout的基础理论
###AL的核心出发点
（1）view具有自我计算尺寸，布局的能力。通过自身的内容，能够获得尺寸。也就是有intrinsic content size的控件（例如button，label）。通俗的讲，具有intrinsic content size的控件自己知道（可以计算）自己的大小，例如一个label，当你设置text，font之后，其大小是可以计算到的。
（2）view的布局位置，确定它与superview及其它view的关系。
（3）与传统的autoresizingmask自适应相比，AL更加精确，精确view的布局位置。
（4）view不一定需要一个初始化的Rect。AL中，View如果有足够的constraint，便能够确定自己的尺寸和位置，并且知道自己和其它view的关系。也就是说，如果想确定view的布局，就该给它（们）添加constraint。
##xib和SB下的AutoLayout
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
##编码方式使用AutoLayout
###VFL
VFL在程序中可以通过
``` bash
+ (NSArray *)constraintsWithVisualFormat:(NSString *)format options:(NSLayoutFormatOptions)opts metrics:(NSDictionary *)metrics views:(NSDictionary *)views;  
```
-它返回一组constraint
-format是你的VFL字符串
-opts:NSLayoutFormatOptions 用于校准两个成员对象之间的关系和方向
-metrics:可以自定义变量，在编译器解析时，自动替换为metrics字典中的value。
-views:需要constraint关系的所有view，也可以是一个（子view和父view）
###VFL例子
写VFL字串的时候，应想象出合理的画面，来保证VFL的合理性，不合理的constraint会直接导致崩溃。
``` bash
    NSDictionary *dict1 = NSDictionaryOfVariableBindings(_boxV,_headerL,_imageV,_backBtn,_doneBtn);  
    NSDictionary *metrics = @{@"hPadding":@5,@"vPadding":@5,@"imageEdge":@150.0};  
    NSString *vfl = @"|-hPadding-[_boxV]-hPadding-|";  
    NSString *vfl0 = @"V:|-25-[_boxV]";  
    NSString *vfl3 = @"V:|-vPadding-[_headerL]-vPadding-[_imageV(imageEdge)]-vPadding-[_backBtn]-vPadding-|";  
```
dict1就是api 中需要的最后一个参数views。由上述宏来完成。

metrics定义了一些vfl中要用的参数。

下面有些vfl字串，一看便知如何使用metrics。

看到：

1)"|"表示superview. 

|-间距-[view1对象名]-(>=20)-[view2对象名]

不写H/V就表示横向，间距可以写固定值也可写>/<。

形象化的理解，"|"是用来确定view上、下、左、右关系的。

想要确定从上到下的关系，就加V:|。那么这个vfl字串就可以描述从上到下的view们的关系。

2)看到vfl3里面，方括号表示view，圆括号表示尺寸数值。支持大小等于。或者另一个view　|-[view1(view2)]，v１的宽度等于v２。

3)优先级用＠表示。如V:|-50@750-[view(55)]，或者写到metrics里面更好。

具体定义查看UILayoutPriority。有几个固定的数值。1000表示必须支持。

4)options，这个要看具体需要。如果是竖排V布局，可以添加NSLayoutFormatAlignAllLeft，让他们对齐。

根据需要也可以添加按位或NSLayoutFormatAlignAllLeft | NSLayoutFormatAlignAllRight。（鬼知道什么需要，自己看经验吧）

5)写好以后一般把constraint添加给superview：