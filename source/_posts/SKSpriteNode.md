title: SKSpriteNode
date: 2014-11-15 09:47:02
tags: SpriteKit
---
[SKSpriteNode](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKSpriteNode_Ref/)和Cocos2d中的精灵相似。可以用图片或者[SKTexture](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKTexture_Ref/)对象来初始化SKSpriteNode对象。而SKTexture可以通过创建[SKView](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKView/),使用下面的方法返回一个SKTexture对象，参数为[SKNode](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKNode_Ref/)也就是说可以以任何一个SKNode对象创建一个SKTexture贴图对象，如[SKLabelNode](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKLabelNode_Ref/)详细的以后再说。

``` bash
$ - (SKTexture *)textureFromNode:(SKNode *)node;
``` 

## 快速入门

### 创建一个SKSpriteNode
1、创建一个有颜色的矩形区域为精灵
2、通过贴图创建精灵
3、通过图片创建精灵

``` bash
$ + (instancetype)spriteNodeWithColor:(SKColor *)color size:(CGSize)size;
$ - (instancetype)initWithTexture:(SKTexture *)texture;
$ + (instancetype)spriteNodeWithImageNamed:(NSString *)name;
``` 
例子
