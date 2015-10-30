---
layout: post
title:  设置app的状态栏样式
tags: iOS9 status bar
eye_catch:
---


* 状态栏的字体为黑色： UIStatusBarStyleDefault
* 状态栏的字体为白色： UIStatusBarStyleLightContent

> 设置app的状态栏样式的使用使用了旧的方式，在info.plist里面设置了View controller-based status bar appearance为NO，默认为YES，一般式iOS6的时候使用这种方式，iOS7，8也兼容，但是到了iOS9就报了警告。
一. 在 info.plist 中，将 View controller-based status bar appearance 设为 NO
状态栏字体的颜色只由下面的属性设定，默认为白色：

```
// default is UIStatusBarStyleDefault
[UIApplication sharedApplication].statusBarStyle
```
<!--more-->
解决个别 vc 中状态栏字体颜色不同的办法
1、在info.plist中，将View controller-based status bar appearance设为NO.
2、在app delegate中：

```
[UIApplication sharedApplication].statusBarStyle = UIStatusBarStyleLightContent;
```
3、在个别状态栏字体颜色不一样的vc中

```
-(void)viewWillAppear:(BOOL)animated{
	[UIApplication sharedApplication].statusBarStyle = UIStatusBarStyleDefault;
}
-(void)viewWillDisappear:(BOOL)animated
{
	[super viewWillDisappear:animated];
	[UIApplication sharedApplication].statusBarStyle = UIStatusBarStyleLightContent;
}
```
> 以前我们通过上面代码改变状态了颜色，iOS9以后点进去看api发现如下说明

```
// Setting the statusBarStyle does nothing if your application is using the default UIViewController-based status bar system.
@property(readwrite, nonatomic) UIStatusBarStyle statusBarStyle NS_DEPRECATED_IOS(2_0, 9_0, "Use -[UIViewController preferredStatusBarStyle]");
- (void)setStatusBarStyle:(UIStatusBarStyle)statusBarStyle animated:(BOOL)animated NS_DEPRECATED_IOS(2_0, 9_0, "Use -[UIViewController preferredStatusBarStyle]");
```
解决办法：
修改方式将View controller-based status bar appearance设置为YES，然后使用新的方式来实现状态栏的样式。

```
- (UIStatusBarStyle)preferredStatusBarStyle;
- (UIViewController *)childViewControllerForStatusBarStyle;
- (void)setNeedsStatusBarAppearanceUpdate
```
View controller-based status bar appearance的默认值就是YES。
如果View controller-based status bar appearance为YES。
则[UIApplication sharedApplication].statusBarStyle 无效。
用下面的方法：
1、在vc中重写vc的preferredStatusBarStyle方法。

```
-(UIStatusBarStyle)preferredStatusBarStyle
{
return UIStatusBarStyleDefault;
}
```
2、在viewDidload中调用：[self setNeedsStatusBarAppearanceUpdate];
但是，当vc在nav中时，上面方法没用 ，vc中的preferredStatusBarStyle方法根本不用被调用。
原因是，[self setNeedsStatusBarAppearanceUpdate]发出后，
只会调用navigation controller中的preferredStatusBarStyle方法，
vc中的preferredStatusBarStyley方法跟本不会被调用。
解决办法有两个：
方法一：
设置navbar的barStyle 属性会影响status bar 的字体和背景色。如下。
//status bar的字体为白色
//导航栏的背景色是黑色。

```
self.navigationController.navigationBar.barStyle = UIBarStyleBlack;
```

//status bar的字体为黑色
//导航栏的背景色是白色，状态栏的背景色也是白色。

```
//self.navigationController.navigationBar.barStyle = UIBarStyleDefault;
```

方法二：
自定义一个nav bar的子类，在这个子类中重写preferredStatusBarStyle方法：

```
Nav* nav = [[Nav alloc] initWithRootViewController:vc];
self.window.rootViewController = nav;
@implementation Nav
- (UIStatusBarStyle)preferredStatusBarStyle
{
UIViewController* topVC = self.topViewController;
return [topVC preferredStatusBarStyle];
}
```
