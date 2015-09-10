---
layout: post
title: Android笔记：Android入门
description: 前端相关收藏。
tags: 后端
---

Android是一种基于Linux的自由及开放源代码的操作系统，主要使用于移动设备，如智能手机和平板电脑，由Google公司和开放手机联盟领导及开发。

## **Android平台技术架构**

Android平台采用了软件堆层的架构，主要分为四个部分：

* 底层以Linux核心为基础，并包含各种驱动，只提供基本功能；
* 中间层包括程序库和Android运行时环境；
* 再往上一层是Android提供的应用程序框架层；
* 最上层是各种应用软件，包括打电话、发短信等程序，由应用软件开发人员自行开发。

<p class="picture"><img alt="" src="{{site.qiniu_static}}/assets/img/2015-9-10/framework.jpg"/></p>

### **应用程序**

Android会附带一系列核心应用程序包，这些应用程序包包括E-mail客户端、SMS短信程序、日历、地图、浏览器、联系人管理程序等。Android中所用的应用程序都是由Java语言编写的。

### **应用程序框架**

开发者也可以访问Android应用程序框架中的API。该应用程序架构简化了组件的组件的重用，任何一个应用程序都可以发布它的功能块，并且任何其他的应用程序都可以使用这些发布的功能块。同样，该应用程序的重用机制也使用户可以方便的替换程序组件。

隐藏在每个应用程序后面的是Android提供的一些列的服务和管理器，其中包括：

* Views：包括Lists、Grids、Text Boxes、Buttons，甚至包括可嵌入的Web浏览器，这些视图可以用来构建应用程序；
* Content Providers：使得应用程序可以访问另一个应用程序的数据（例如，联系人数据库），或者可以共享它们自己的数据；
* Resource Manager：提供非代码资源的访问，例如本地字符串、图形和布局文件等；
* Notification Manager：使用应用程序可以在状态栏中显示自定义的提示信息；
* Activity Manager：用来管理应用程序生命周期，并且提供常用的导航会退功能。

### **程序库**

Android平台包含一些C/C++库，Android系统中的组件可以使用这些库。它们通过Android应用程序框架为开发者提供服务。这些程序库主要包括：

* 系统C库：一个从BSD继承的标准C系统函数库，它是专门为基于嵌入式Linux设备定制的；
* 媒体库：基于PacketVideo的OpenCORE，该库支持多种常用的音频、视频格式文件的回放和录制，同时支持静态图像文件，编码格式包括MPEG4，H.264、MP3、AAC、AMR、JPG和PNG等；
* Surface Manager：管理显示子系统，并且为多个应用程序提供2D和3D图层的无缝融合；
* LibWebCore：一个最新的Web 浏览器引擎，支持Android浏览器和一个可嵌入的Web视图；
* SGL：底层的2D图形引擎；
* 3D库：基于OpenGL ES 1.0 API实现，该库可以使用3D硬件加速或者使用高度优化的3D软加速；
* FreeType：用于位图和矢量字体显示；
* SQLite库：一个对于所用应用程序可用的、功能强劲的轻型关系型数据库引擎。

### **Android运行时环境**

Android运行是环境由一个核心库和Dalvik虚拟机组成。核心库提供Java编程语言核心库的大多数功能。每一个Android应用程序都在自己的进程中运行，都有一个独立的Dalvik虚拟机实例。

Dalvik被设计成一个设备可以同时高校的运行多个虚拟系统。它依赖于Linux内核的一些功能，例如线程机制和底层内存管理机制等。

Dalvik虚拟机执行.dex的Dalvik的可执行问价，该格式文件针对小内存的使用进行了优化，同时虚拟机是基于寄存器的，所有的类由Java编译器编译，然后通过SDK中的“dx”工具转化成.dex格式，最后由虚拟机执行。

### **Linux内核**

Android核心系统服务依赖于Linux内核，如安全性、内存管理、进程管理、网络协议栈和驱动模型等。Linux内核也同时作为硬件和软件栈之间的抽象层。

## **Android网上资源**

* [Android官网](http://www.android.com)
* [Android开发者](http://www.androidin.com)
* [Android开发网](http://www.android123.com)
* [Android中文网](http://www.androidcn.net)
* [Android中文API](http://www.android-doc.com/index.html)
* [Android开发资源下载](http://www.androidhere.cn)
* [Cordova官网](http://cordova.apache.org)
* [CrossWalk官网](https://crosswalk-project.org)
