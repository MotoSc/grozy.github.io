title: 摘记-如何使Swift方法返回值为空
date: 2014-11-15 20:32:35
tags: [摘记,Swift]
---
今天在写基类的时候，打算子类实现对象，但是发现不能像在iOS中在父类方法中直接返回一个空值了。
##iOS中:
``` bash
+ (SKTexture *)generateTexture
{
  // Overridden by subclasses
  return nil;
}
```
##Swift中:
``` bash
func generateTexture() -> SKTexture
{
        return nil
}
```
![警告](/img/摘记-如何使Swift方法返回值为空/warn.png)
