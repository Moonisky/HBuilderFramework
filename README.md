# HBuilderFramework

将 HBuilder（HTML 5+ Runtime）静态库集中打包为 Cocoa Touch Framework，快速集成 HBuilder 框架。

## 缘由

在使用 HBuilder 进行 Hybrid 开发的时候，往往要集成海量的 `.a` 库和关联的系统 `Framework`，而官方提供的 Demo 工程具备以下几个特点：

1. Demo 基本使用 Objective-C 编写，Swift 工程则集成了很多无用、容易令人混淆的功能；
2. 在使用新工程进行集成的时候，很容易漏掉相关的静态库和框架；
3. 海量的静态库和框架很容易污染 Linked Framework and Libraries，看着头晕，而且如果集成了其他第三方框架的时候，很容易出现混淆；
4. 官方提供的 Demo 工程糅合了大量可能无用的第三方框架，例如推送、统计、地图、语音等等。

因此，我们便有了一个想法，是否能够将这些海量的 `.a` 库和关联的系统动态库集中糅合在一个框架里面呢，答案是可行的，因此也就有了这个框架的来历。

这个简单的框架已经在鄙人单位的 App 中稳定运行了半年，可靠性和可用性基本能够得到保障，大家可以放心使用。

如果您发现了 BUG 或者问题，欢迎提 issue！

## 声明

本框架仅仅只是对 HBuilder 的 HTML 5+ Runtime SDK 框架进行了封装集成，框架版权为 [DCloud](http://www.dcloud.io/) 公司所有。所有框架资源包均从 DCloud 提供的[下载界面](http://ask.dcloud.net.cn/article/103)公开下载。

目前使用的版本号为：*1.9.9.44932*。

## 使用方式

您可以参考提供的 Demo 来查看引入的方式。

### Framework 导入

Framework 导入是让工程界面最清爽的办法，但是相对来说比较复杂。

您可以前往 [Release 界面](https://github.com/Moonisky/HBuilderFramework/releases)下载我预先提供好的 Framework。

您也可以将本工程克隆至本地，然后去 [SDK 下载界面](http://ask.dcloud.net.cn/article/103)下载最新版本的 iOS SDK，将下载好的 SDK 解压之后，将 SDK 文件夹中的 SDK > Libs 目录，拷贝到该工程的 HBuilderFramework > Pandora 目录下。然后打开此工程，选择 Aggregate Scheme 进行编译。

> 我编写了对应的脚本，保证编译出来的 Framework 能够同时支持模拟器和真机。编译完成后会自动打开 Framework 所在的目录。

打开对应 Target 的 General 选项卡，在 Linked Frameworks and Libraries 选项中点击 + 号键。在弹出的框架添加界面中，选择 Add Other…，然后将准备好的 Framework 添加到您的 iOS 工程当中。

至此，我们就完成了工程的引用，您可以在 iOS 工程中尽情使用封装好的 HTML 5+ Runtime SDK 了。

### 工程引用

工程引用是最简单的方法，它兼顾了模块自定义的需求，也为项目的集成编译打包提供了便利，省却了之前直接导入 Framework 包带来的种种麻烦。

将本工程克隆至本地，然后去 [SDK 下载界面](http://ask.dcloud.net.cn/article/103)下载最新版本的 iOS SDK，将下载好的 SDK 解压之后，将 SDK 文件夹中的 SDK > Libs 目录，拷贝到该工程的 HBuilderFramework > Pandora 目录下。

随后将本工程的 `HBuilderFramework.xcodeproj` 文件拖到您 iOS 工程的任一位置。

打开对应 Target 的 General 选项卡，在 Linked Frameworks and Libraries 选项中点击 + 号键。在弹出的框架添加界面中，选择 Workspace > HBuilderFramework。

至此，我们就完成了工程的引用，您可以在 iOS 工程中尽情使用封装好的 HTML 5+ Runtime SDK 了。

## 自定义模块集成

待续。
