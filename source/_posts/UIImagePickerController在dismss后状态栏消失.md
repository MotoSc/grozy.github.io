title: UIImagePickerController在dismss后状态栏消失
date: 2014-11-20 13:50:41
tags: [iOS,UIImagePickerController]
---
在做iOS上传头像的时候，自定义了图像的裁剪的ViewController。按照需求，要求和系统的一样，从相机或者相册直接push到编辑图像的界面。但是当我直接push图像裁剪的ViewController中之后在进行dismiss，结果状态栏消失不见了。在网上找到了一个能显示状态栏的方法,先隐藏状态栏之后在显示。
```bash
//处理调用UIImagePickerViewController push到自定义裁剪的ViewController中后，statusBar消失
- (void)showStatusBar
{
    [UIView animateWithDuration:0.4 animations:^{

        [[UIApplication sharedApplication] setStatusBarHidden:YES];
    
    } completion:^(BOOL finished) {

        [[UIApplication sharedApplication] setStatusBarHidden:NO];

    }];
}
```