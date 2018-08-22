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

本工程只是一个简单的示例，只集成了 HBuilder 基础包，因此如果您如果还需要添加其他模块，那么可以很轻松地进行自定义集成。

您只需要根据官方 SDK 的 Feature-iOS.xls 文件，查询您需要引入的模块，然后将对应的静态库和系统动态框架加入到 HBuilderFramework 的 Linked Frameworks and Libraries 选项中即可。

## 自建框架

HBuilderFramework 框架其实非常简单，您也可以自行建立。

通过 Xcode 新建一个 Cocoa Touch Framework，然后输入相关的信息。

随后，将 SDK 当中的 `inc` 目录拖入到框架当中。

为了方便，建议您将 SDK 中的 `Libs` 目录放到工程目录当中，方便寻找和引用。

找到 Build Settings 选项卡，向 Other Linker Flags 中添加 `-ObjC` 标记。

找到框架自带的 `.h` 文件（一般是框架名称），加入以下两句头文件引入语句：

```objective-c
#import "PDRCore.h"
#import "PDRToolSystemEx.h"
```

找到 Build Phases 选项卡，将 `PDRToolSystemEx.h`、`PDRCore.h`、`PDRCoreSettings.h` 和 `PDRCoreDefs.h` 文件从 Project 拖入到 Public 域当中，这样才能够对外暴露使用。

随后，根据官方 SDK 的 Feature-iOS.xls 文件，查询您需要引入的模块，然后将对应的静态库和系统动态框架加入到 HBuilderFramework 的 Linked Frameworks and Libraries 选项中即可。

### 自动打包&合并

上述创建出来的框架只能够打出适用于模拟器或者真机的动态框架包，在真实使用中，我们还需要进行合并。

如果您采用的是*工程引用*的方式使用本框架，那么可以忽略这个操作。

但是如果是直接采用*Framework 导入*的方式使用，那么还需要将生成的两个动态框架包进行合并。但是合并的操作比较麻烦，我们可以采用创建 Aggregate 的方式自动执行 Shell 脚本，快速帮助我们完成打包、合并。

通过 Xcode 创建 Aggreagate Target，然后进入到 Aggregate 的 Build Phases 选项卡，新建一个 Run Script Phase，输入以下内容：

```shell
#!/bin/sh
#要 build 的 target 名
TARGET_NAME=${PROJECT_NAME}
if [[ $1 ]]
then
TARGET_NAME=$1
fi
UNIVERSAL_OUTPUT_FOLDER="${SRCROOT}/Products/"

#创建输出目录，并删除之前的 framework 文件
mkdir -p "${UNIVERSAL_OUTPUT_FOLDER}"
rm -rf "${UNIVERSAL_OUTPUT_FOLDER}/${TARGET_NAME}.framework"

#分别编译模拟器和真机的 Framework
xcodebuild -target "${TARGET_NAME}" ONLY_ACTIVE_ARCH=NO -configuration ${CONFIGURATION} -sdk iphoneos BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}" clean build
xcodebuild -target "${TARGET_NAME}" ONLY_ACTIVE_ARCH=NO -configuration ${CONFIGURATION} -sdk iphonesimulator BUILD_DIR="${BUILD_DIR}" BUILD_ROOT="${BUILD_ROOT}" clean build

#拷贝 framework 到 univer 目录
cp -R "${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${TARGET_NAME}.framework" "${UNIVERSAL_OUTPUT_FOLDER}"

#合并 framework，输出最终的 framework 到 build 目录
lipo -create -output "${UNIVERSAL_OUTPUT_FOLDER}/${TARGET_NAME}.framework/${TARGET_NAME}" "${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${TARGET_NAME}.framework/${TARGET_NAME}" "${BUILD_DIR}/${CONFIGURATION}-iphoneos/${TARGET_NAME}.framework/${TARGET_NAME}"

#删除编译之后生成的无关的配置文件
dir_path="${UNIVERSAL_OUTPUT_FOLDER}/${TARGET_NAME}.framework/"
for file in ls $dir_path
do
if [[ ${file} =~ ".xcconfig" ]]
then
rm -f "${dir_path}/${file}"
fi
done

#判断 build 文件夹是否存在，存在则删除
if [ -d "${SRCROOT}/build" ]
then
rm -rf "${SRCROOT}/build"
fi
rm -rf "${BUILD_DIR}/${CONFIGURATION}-iphonesimulator" "${BUILD_DIR}/${CONFIGURATION}-iphoneos"

#打开合并后的文件夹
open "${UNIVERSAL_OUTPUT_FOLDER}"
```

即可完成自动将模拟器和真机的包打出，并自动合并。