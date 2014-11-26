title: AutoLayout中Content Hugging和Content Compression Resistance
date: 2014-11-26 13:53:31
tags: [AutoLayout,Content Hugging,Content Compression Resistance]
---
##Content Hugging 和 Content Compression Resistance
这两个属性对有intrinsic Content size 的控件（如UIButton，UILabel）非常重要。通俗的讲，具有intrinsic content size的控件自己知道（可以计算）自己的大小，例如一个label，当你设置text，font之后，其大小是可以计算到的。关于intrinsic content size官方的解释：
``` bash
Custom views typically have content that they display of which the layout system is unaware. Overriding this method allows a custom view to communicate to the layout system what size it would like to be based on its content. This intrinsic size must be independent of the content frame, because there’s no way to dynamically communicate a changed width to the layout system based on a changed height, for example.
```
好了，了解了intrinsic content size的概念之后，下面就重点讨论Content Hugging 和 Content Compression Resistance了。
UIView中关于Content Hugging 和 Content Compression Resistance的方法有：
``` bash
- (UILayoutPriority)contentCompressionResistancePriorityForAxis:(UILayoutConstraintAxis)axis
- (void)setContentCompressionResistancePriority:(UILayoutPriority)priority forAxis:(UILayoutConstraintAxis)axis
- (UILayoutPriority)contentHuggingPriorityForAxis:(UILayoutConstraintAxis)axis
- (void)setContentHuggingPriority:(UILayoutPriority)priority forAxis:(UILayoutConstraintAxis)axis
```
优先级常量
``` bash
typedef float UILayoutPriority;
static const UILayoutPriority UILayoutPriorityRequired NS_AVAILABLE_IOS(6_0) = 1000; // A required constraint.  Do not exceed this.
static const UILayoutPriority UILayoutPriorityDefaultHigh NS_AVAILABLE_IOS(6_0) = 750; // This is the priority level with which a button resists compressing its content.
static const UILayoutPriority UILayoutPriorityDefaultLow NS_AVAILABLE_IOS(6_0) = 250; // This is the priority level at which a button hugs its contents horizontally.
static const UILayoutPriority UILayoutPriorityFittingSizeLevel NS_AVAILABLE_IOS(6_0) = 50; // When you send -[UIView systemLayoutSizeFittingSize:], the size fitting most closely to the target size (the argument) is computed.  UILayoutPriorityFittingSizeLevel is the priority level with which the view wants to conform to the target size in that computation.  It's quite low.  It is generally not appropriate to make a constraint at exactly this priority.  You want to be higher or lower.
```
Hugging priority 确定view有多大的优先级阻止自己变大。

Compression Resistance priority确定有多大的优先级阻止自己变小。

很抽象，其实content Hugging就是要维持当前view在它的optimal size（intrinsic content size），可以想象成给view添加了一个额外的width constraint，此constraint试图保持view的size不让其变大：

view.width <= optimal size

此constraint的优先级就是通过上面的方法得到和设置的，content Hugging默认为250.

Content Compression Resistance就是要维持当前view在他的optimal size（intrinsic content size），可以想象成给view添加了一个额外的width constraint，此constraint试图保持view的size不让其变小：
view.width >= optimal size
此默认优先级为750.
``` bash
    button1 = [UIButton buttonWithType:UIButtonTypeRoundedRect];  
    button1.translatesAutoresizingMaskIntoConstraints = NO;  
    [button1 setTitle:@"button 1 button 2" forState:UIControlStateNormal];  
    [button1 sizeToFit];  

    [self.view addSubview:button1];  

    NSLayoutConstraint *constraint = [NSLayoutConstraint constraintWithItem:button1 attribute:NSLayoutAttributeLeading relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeLeading multiplier:1.0f constant:100.0f];  
    [self.view addConstraint:constraint];  

    constraint = [NSLayoutConstraint constraintWithItem:button1 attribute:NSLayoutAttributeTop relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeTop multiplier:1.0f constant:100.0f];  
    [self.view addConstraint:constraint];  

    constraint = [NSLayoutConstraint constraintWithItem:button1 attribute:NSLayoutAttributeTrailing relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeTrailing multiplier:1.0f constant:-150.0f];  
    constraint.priority = 751.0f;  
    [self.view addConstraint:constraint];  
```
创建一个按钮，设置其constraint，使其距离顶部100，距离左边100，距离右边150，注意右边constraint的优先级设置为751.0（priority的默认值为UILayoutPriorityRequired = 1_000），此时AutoLayout系统会先去首先满足constraint，然后再去满足Content compression resistance(UILayoutPriorityDefaultHigh = 750)。
当将751改变为749(priority < UILayoutPriorityDefaultHigh)时，AutoLayout优先满足Content compression resistance，而之后对constraint做处理。也就是说内容最小压缩到完全呈现内容的大小，上限受constraint控制。
（图片）
这就是先满足了Content compression resistance 的constraint，因为其优先级高
下面举例Content Hugging，例码如下：
``` bash
    button1 = [UIButton buttonWithType:UIButtonTypeRoundedRect];  
    button1.translatesAutoresizingMaskIntoConstraints = NO;  
    [button1 setTitle:@"button 1 button 2" forState:UIControlStateNormal];  
    [button1 sizeToFit];  

    [self.view addSubview:button1];  

    NSLayoutConstraint *constraint = [NSLayoutConstraint constraintWithItem:button1 attribute:NSLayoutAttributeLeading relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeLeading multiplier:1.0f constant:100.0f];  
    [self.view addConstraint:constraint];  

    constraint = [NSLayoutConstraint constraintWithItem:button1 attribute:NSLayoutAttributeTop relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeTop multiplier:1.0f constant:100.0f];  
    [self.view addConstraint:constraint];  

    constraint = [NSLayoutConstraint constraintWithItem:button1 attribute:NSLayoutAttributeTrailing relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeTrailing multiplier:1.0f constant:-10.0f];  
    constraint.priority = 251.0f;  
    [self.view addConstraint:constraint];  
```
