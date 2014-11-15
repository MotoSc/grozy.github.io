title: 摘记-如何使Swift方法返回值为空
date: 2014-11-15 20:32:35
tags: [摘记,Swift]
---
今天在写基类的时候，打算子类实现对象，但是发现不能像在iOS中在父类方法中直接返回一个空值。
![警告](/img/摘记-如何使Swift方法返回值为空/warn.png)
还没等着编译就给我抛出了个warn。我这时候想起，之前遇到过的这个问题了，关于之前区分?和!的区别。
``` bash
var num :Int = nil //抛出的错误和我今天遇到的一样。
var num :Int? = nil //当加上？之后问题就解决了。
```
撇个[Optional](http://blog.csdn.net/zhangao0086/article/details/38640209)有关的传送门。大概就是这么个意思，声明一个对象的时候，Swift不会像obj-C和其他语言一样为你默认的设置一个为nil的初始值。在Swift中，没有初始值的变量是不可以使用的，所以出现了Optional类型。在下面的类方法的返回值后面，加一个问号就可以让返回类型支持NilLiteralConvertible这个协议。
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
//解决办法
func generateTexture() -> SKTexture?
{
    return nil
}
```
