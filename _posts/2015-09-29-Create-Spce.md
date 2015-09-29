
---
layout: post
title: Create Space
tags: CocoaPods ruby
eye_catch: http://jekyllrb.com/img/logo-2x.png
---

本文CcocaPods来管理依赖库，没有使用模板来创建，未完待续......

更详细的请看-：[CocoaPods](https://cocoapods.org/)


# 使用CocoaPods  创建依赖库

1.在github 上面创建工程，

	*_github上面创建私有项目需要收费，这里创建一个公开项目（创建README,添 加.gitignore,license）_*
	* 仓库的公开性，public
	* README:说明文档
	* .gitignore:记录了文件类型，git管理时不会将其纳入版本管理中
	* license ：许可证，使用cocopods 必须要有

<!--more-->

2.将仓库clone到本地

	git clone "xxxxxxxxx.git"

3.在本地git仓库中添加创建Pods依赖库所需文件

>  pod spec create  xxxx

```
Pod::Spec.new do |s|
  s.name         = "xxxx"
  s.version      = "1.0.0"
  s.summary      = "xxxx"
  s.description  = <<-DESC
                    xxxxx
                   DESC
  s.homepage     = "https://github.com/xxxxxxl"

  s.license      = 'MIT'
  s.author       = { "xxxxxx" => "xxxxx@gmail.com" }
  s.social_media_url = "http://twitter.com/xxxxx"
  s.source       = { :git => "https://github.com/xxxxx/xxxxx.git", :tag => s.version.to_s }

  s.platform     = :ios, '6.0'
  s.requires_arc = true

  s.source_files = 'xxxx/*'
  s.frameworks = 'Foundation', 'UIKit'
  #s.private_header_files = 'Classes/ios/private/*.h'

  #s.dependency 'AutoLayout', '~> 0.0.1'
end
```
4.使用trunk上传

```
pod trunk register xxxx@xx.com "xxx" -- description = 'xxxx'  --verbose
加上--verbose可以输出详细debug信息，方便出错时查看。
注册后CocoaPods会给你的邮箱发送验证链接，点击后就注册成功了，可以用pod trunk me命令查看自己的注册信息

```
5.验证和上传 podspec文件到trunk

```
	tag一个版本号并发布一个release版本，这样podspec文件中的s.source的值才能是准确的
	$ git add -A && git commit -m "Release 1.0.0."
	$ git tag '1.0.0'
	$ git push --tags
	$ git push origin master
	pod添加版本号并打上tag
	$ set the new version to 1.0.0
	$ set the new tag to 1.0.0
```
6.pod trunk push xxxx.podspec


```
pod trunk push命令做了如下三个工作：
验证你本地的podspec文件（你也可以用pod lib lint命令来验证）
上传你的podspec文件到trunk
将你的podspec文件转化成trunk需要的JSON文件

```
7.运行pod setup来更新你的Pods依赖库tree后，再使用pod search




> ps:如果在工程里面添加了测试工程，每次添加新的Class/修改xxxx.podspec，都要把测试工程pod update/pod install.




