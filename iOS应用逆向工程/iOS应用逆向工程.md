#### iOS应用逆向工程

- (高)维度攻击
- 作用
  - 分析目标程序，拿到关键信息，可以归类于安全相关的逆向工程
  - 借鉴他人的程序功能来开发自己的软件，可以归类于开发相关的的你想工程。
- 应用
  - 评定安全等级
  - 逆向恶意软件
  - 检查软件后门
  - 去除软件使用限制


- 攻城狮编写的软件之所以能够运行在操作系统中，提供各种各样的功能，是因为操作系统本身已经内嵌了这个功能，软件知识对其进行重新组合罢了。
- 一般来说，软件你想工程可以看做系统分析和代码分析两个阶段的有机结合。
- 逆向工具
  - 监测工具(Reveal)
  - 反汇编工具(IDA和Hopper)
  - 调试工具(LLDB)
  - 开发工具(Theos)
- 相较于iOS应用的高层表象，人们对其底层实现更感兴趣，这也是大家进行逆向工程的源动力。
- 纯粹的AppStore开发组可能对iOS系统结构一无所知。
- iOS一旦越狱，来自Cydia的App就可以拥有比StoreApp更高的权限，从而访问全系统文件。
- 文件目录
  - /Applications：存放所有的系统App和来自于Cydia的App，不包括StoreApp
  - /Developer：如果一台设备连接Xcode制定为调试机，Xcode就会在iOS中生成这个目录，其中会含有一些调试需要的工具和数据。
  - /Library：存放一些提供系统支持的数据。
  - /System/Library： iOS文件中最重要的目录之一，存放大量系统组件。
  - /System/Library/Frameworks和/System/Library/PrivateFrameworks：存放iOS中的各种framework，其中出现在SDK文档里的只是冰山一角。
  - /System/Library/CoreServices里SpringBoard.app:iOS桌面管理器(类似于Windows的Exolorer)，使用户与系统交流的最重要的中介。
  - 。。。
- iOS是一个多用户操作系统。"用户是一个抽象的概念，它代表对操作系统的所有权和使用权"。
- iOS用3位来表示文件权限，从高位到低位分别是r(read)，w(write)，以及x(execute)权限。111101101代表rwxr-xr-x，即代表属主有rwx权限，而属主组里面的用户和其他用户只有rx权限。
- Dynamic Library(以下简称dylib)和Daemon这3类二进制文件，对他们的了解越深入，逆向工程就会越顺利。
- bundle
  - bundle的概念来源于NeXTSTEP，他不是一个文件，而是一个按某种标准结构来组织的目录。正向开发中常见的App和Framework都是以bundle的形式存在的;在越狱iOS常见的PreferenceBundle，可以看成是一种依附于Settings的App,结构与App类似，本质也是bundle。
- lproj目录下存放的是各种本地化的字符串(.string)，是iOS你想工程的重要线索，也可以用plutil查看。
- /Applications/目录存放系统App和从Cydia下载的App(我们把来自Cydia的App)，而/var/mobile/Containers/目录存放的则是StoreApp。
- 沙盒会将App的文件访问范围限制在App内部，一个App不知道其他App的存在，更别说访问它们了，沙盒还会限制App的功，；例如对iCloud接口的调用就必须经过沙盒的允许。