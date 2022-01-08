---
title: Flutter开发环境搭建
date: 2020-09-01 11:42:11
categories: Flutter
tags: 环境搭建
---

## 安装Flutter SDK

[官网地址](https://flutter.dev/docs/development/tools/sdk/releases?tab=macos#macos) 可能需要科学上网

此文章针对 mac 系统进行配置,如需要配置window环境,可以查看 [技术胖](https://jspang.com/detailed?id=41)的博客 或者去 [Flutter中文网](https://flutterchina.club/setup-macos/)查看教程。

本人在配置过程中遇到最多的问题就是网络问题,建议科学上网操作一下全部流程。

### SDK下载完成

1. 解压安装包存放到你想放置的文件内,注意这一步很重要,因为在接下来的环境变量配置需要用到它. 例如: 我在Dektop下新建了一个Flutter文件夹存放解压后的Flutter SDK

2. 配置环境变量: 打开终端 ls -a 查看是否有 .bash_profile 文件, 如果没有的话新建, 有的话打开该文件(打开的方式有很多,怎么方便怎么来吧)。

                新建文件的命令是: touch 文件名

3. 打开或者新建了文件之后编写文件

                export PUB_HOSTED_URL=https://pub.flutter-io.cn  # 国内用户需要设置
                export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn  # 国内用户需要设置
                export PATH=SDK-Path/flutter/bin:$PATH

    注意: SDK-Path 为你flutter的路径，比如我刚刚在Desktop下新建了Flutter文件夹存放,那么我的SDK-Path 这里应该是 /Users/你的电脑用户名/Desktop/Flutter , 如果环境变量没有配置成功回来这里检查就对了. (如果想要快速查看这个文件存放位置, 可以进入该文件夹 打开终端输入pwd 查看)
    如果你的mac终端是 zsh 那么你肯定知道 终端启动时 ~/.bash_profile 将不会被加载，解决办法就是修改 ~/.zshrc ，在其中添加：source ~/.bash_profile

4. 上一步配置完成后,重启终端或者运行 source \$HOME/.bash_profile 刷新当前终端窗口.

5. 到了这里Flutter SDK算是安装完成了,并且你可以在随处打开终端执行 flutter 命令了。

6. 验证是否安装成功:

                flutter -h

7. 如果环境配置成功会有输出,接下来检查Flutter开发环境, 输入命令:

                flutter doctor

8. 不出所料可能会出现很多的 x , 例如:

                To install, run:
                brew install --HEAD libimobiledevice
                brew install ideviceinstaller
                ✗ ios-deploy not installed. To install:
                    brew install ios-deploy
                ✗ CocoaPods not installed.
                    CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
                    Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
                    For more info, see https://flutter.io/platform-plugins
                To install:
                    brew install cocoapods
                    pod setup

    **备注:**  每台电脑的环境可能会有所不同, 大家根据自己的提示进行一系列安装. 上面输出大概意思就是我们需要这些软件，Flutter推荐你用brew命令进行安装。

9. 安装完以上软件之后, 我们再来安装 Android Studio , 下载地址: <http://www.android-studio.org/>

    安装好之后运行 Android Studio , 然后打开另外终端 输入命令允许协议（android-licenses）:

                flutter doctor --android-licenses

    然后让你输入Y/N的时候，一路Y就可以了.

10. 上面那一步安装好之后 再次输入 flutter doctor 进行环境检测 , 查看是否有这一行信息:

                ✗ Flutter plugin not installed; this adds Flutter specific functionality.
                ✗ Dart plugin not installed; this adds Dart specific functionality.

    如果有 说明你的 Android Studio 没有安装Flutter 插件, 打开 Android Studio 后 快捷键 :

                command + ,

    选择 plugings 后搜索 Flutter 插件安装,安装完成之后重新启动 Android Studio, 然后再次输入检测命令:

                flutter doctor

11. 如果有提示 vscode 的文字, 下载打开vscode ,查找Flutter插件以及dart插件 并安装好它们.

12. 完成vscode的步骤之后, 再次输入命令 flutter doctor 检测环境,如果全部都 [✓] 符号, 只有出现下面这个提示 :

                [!] Connected devices
                    ! No devices available

    那么恭喜你,环境已经搭建好了, 这个提示只是提示没有找到 调试设备, 可以不用管它.

13. 接下来 安装 AVD 虚拟机 , 打开 Android Studio 后界面会出现一个 start a new Flutter project , 不犹豫 意思就是新建一个 Flutter项目 , 其实新建项目也可以使用终端:

                flutter create myapp

    项目新建后我们就可以看到我们的目录结构了,这个不是重点, 我们在 Android Studio 编辑器上找到 Tools 选择 AVD manager 然后会弹出一个界面,如果你没有安装过 AVD虚拟机, 中间会出现创建按钮的.

14. 虚拟机安装好之后我们就要来跑一下我们新建的Flutter项目了, 这里建议使用 vscode 来运行 Flutter 项目, 毕竟轻量级目前最火编辑器之一嘛.

    打开 vscode 选择我们刚刚新建的Flutter 项目 , 打开 AVD 虚拟机 , 终端输入命令检查Android设备是否在运行:

                flutter devices

    如果你开启了 虚拟机 ,终端会输出如下内容:

                1 connected device:

                AOSP on IA Emulator • emulator-5554 • android-x86 • Android 9 (API 28) (emulator)

    注意 每个人安装的虚拟机操作系统不一样,所以内容输出不是一模一样的哦 .

    如果有输出 ,接下来可以执行命令:

                flutter run

    假如你开了多个虚拟机, 你可以通过命令来达到你想要在哪台虚拟机上运行你的app:

                flutter run -d emulator-5554

    **emulator-5554** 这个是虚拟机的ID , 如果你只是运行了一台则可以忽略这段话.

15. 到此为止, Flutter 开发环境已经配置完毕。

### iOS开发环境设置

1. 安装Xcode 9.0

2. 配置Xcode命令行工具以使用新安装的Xcode版本:

                sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer

    以上路径时对于最新版Xcode的路径。如果你需要使用不同的Xcode版本，需要指定相应路径。

3. 确保Xcode许可协议是通过打开一次Xcode或通过命令sudo xcodebuild -license同意过了,接下来就可以使用Xcode，在iOS设备或模拟器上运行Flutter App了。

4. 在终端输入如下命令打开一个iOS模拟器：

                open -a Simulator

5. 打开虚拟机后 运行项目

## 如何将Flutter 项目跑在真机

目前没有尝试 , 推荐个文章给大家看看: <https://www.devio.org/2019/04/03/development-environment-mac/>

## 总结

整个过程耗时 3 个小时 , 因为网络原因导致很多的问题, 希望大家有个好的翻墙工具以及网络环境 来操作 可能时间会快很多
