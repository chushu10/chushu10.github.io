---
layout: post
title: "推荐 Mac OS X 上的代理软件 ShadowsocksX-NG"
date: 2018-10-15
categories: software
mathjax: false
---

最近在学习 Coursera 上的[设计模式课程](https://www.coursera.org/learn/design-patterns)，由于课程有一个大作业（Capstone Project）需要编写 Android 软件，于是不得不把卸载多年的 [Android Studio](https://developer.android.com/studio/) 又安装了起来。

用过 Android Studio 的朋友都知道，在没有代理的情况下，基本上所有的 SDK 工具都需要去国内镜像下载，十分麻烦。幸亏看到了[这篇文章](https://loyea.com/2017/03/15/macOS-android-studio-proxy-not-work-with-shadowsocks/)，终于知道了 ShadowsocksX-NG 这么个 Mac 软件。

之前一直是通过 Homebrew 安装的 sslocal 程序，在终端里敲 `sslocal -c xxx.json` 命令来连接 shadowsocks 服务器的，但放到 Android Studio 上就不好用了，真是不省心的 IDE！

不过，安装了 ShadowsocksX-NG 软件之后，由于它可以设置 HTTP 代理，因此 Android Studio 也能用上 shadowsocks 代理了！主要步骤如下：

1. 安装好 ShadowsocksX-NG，它会像印象笔记等软件一样，显示在 Macbook 的右上角状态栏上（如下图最左侧的 shadowsocks 经典的**纸飞机**图标）：

    ![status-bar](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/assets/img/shadowsocks/status-bar.png)

2. 点击上面的纸飞机图标，再点击 Preferences 进入设置页面：

    ![shadowsocks-x-ng-preferences](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/assets/img/shadowsocks/shadowsocks-x-ng-preferences.png)

3. 选择 HTTP，IP 默认为 127.0.0.1，并配置端口号，如这里配置的是 1087:

    ![shadowsocks-x-ng-preferences-http](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/assets/img/shadowsocks/shadowsocks-x-ng-preferences-http.png)

4. 点击 Android Studio 的 Preferences 进入设置页面：

    ![android-studio-preferences](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/assets/img/shadowsocks/android-studio-preferences.png)

5. 选择左侧的 HTTP Proxy，并选择右侧的 Manual proxy configuration，选择 HTTP 并填写和刚才一样的 IP 和端口号：

    ![android-studio-preferences-http](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/assets/img/shadowsocks/android-studio-preferences-http.png)

就这样，你又能愉快地开始编写和调试安卓程序了：）