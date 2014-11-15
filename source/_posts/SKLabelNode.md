title: SKLabelNode
date: 2014-11-15 13:30:36
tags: [SpriteKit,SKLabelNode]
---
SpriteKit中[SKLabelNode](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKLabelNode_Ref/index.html)相当于iOS开发中的UILabel标签类。用于在游戏中有需要的位置上显示文本内容给用户。
##SKLabelNode快速入门

###SKLabelNode的简单介绍
通常，要使用一个标签node，通过调用labelNodeWithFontNamed:类方法创建一个标签node。然后配置标签node的其他属性，尤其是text属性。标签node的size明确取决于标签node的fontName，fontSie和text属性，下面看看如何去创建一个文本标签:
``` bash
    SKLabelNode *winner = [SKLabelNode labelNodeWithFontNamed:@"Chalkduster"];
    winner.text = @"You Win!";
    winner.fontSize = 65;
    winner.fontColor = [SKColor greenColor];
    winner.position = CGPointMake(CGRectGetMidX(self.bounds),
                                 CGRectGetMidY(self.bounds));
    [self addChild:winner];
```
不论你如何改变node的属性，标签node会为下次scene的渲染自动的更新。

##SKLabelNode的使用
###创建一个标签node
initWithFontNamed:
用于初始化一个标签对象。
``` bash
$ - (instancetype)initWithFontNamed:(NSString *)fontName
```
``` bash
$ + (instancetype)labelNodeWithFontNamed:(NSString *)fontName
```
fontName:选择标签使用的字体,返回一个标签node对象
position:标签位置
fontColor:字体的颜色
fontSize:字体的大小
text:标签显示的内容
horizontalAlignmentMode:文字的水平位置
verticalAlignmentMode:文字的垂直位置