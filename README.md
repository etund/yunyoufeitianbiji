## 细节

#### @synthesize详解

#### nsoptions nsenum区别

#### NSDictionay setObject forKey与 setValue forKey

#### CRC算法

### UI

#### iOS本地化只显示key的名称



#### 单例模式
- 类方法要都要使用once代码
- 实例方法只需要返回已经声明的实例边变量就可以


#### 模仿MJExtension写一套框架
- 核心
    - 运行时的应用

#### 静态库开发
- 最近新公司开发的项目是静态库，于是就趁机巩固一下本来就不熟悉的静态库开发吧
- 简介
    - 什么是库：库是程序代码的集合，是共享程序代码的一种方式
    - 库的分类
        - 开源库
        - 闭源库
- 静态库和动态库
    - 静态库和动态库的存在形式
        - 静态库：.a 和.framework
        - 动态库：.dylib 和 framework
    - 静态库和动态库在使用上的区别
        - 静态库：链接时，静态库会被完整地复制到可执行的文件中，被多次使用就有多分荣誉拷贝
        - 动态库：链接时不复制，程序运行时由系统动态加载到内存，工程需调用，系统之家在一次，多个程序公用，节省内存
    `注意:项目中如果使用了自制的动态库，不能上传AppStore`
- 静态库的制作
    -

#### 在不使用AutoLayout的情况下什么时候设置Frame最适合
- 解决方案，尽量使用AutoLayout

#### iOS逆向工程

#### Swift学习
- 基本属性
    - 添加成员变量`var animator: UIDynamicAnimator?`
    - 懒加载：

- 新属性
    - 元组
- 代理
- 通知
- block
- AutoLayout


#### OC动态特征
- OC无参数的返回值的方法会被视为get方法，可以用点语法操作

#### iOS状态栏显示不出来
- 最近接手一个新项目，一个静态库的开发，前几天跟老大算是忙前忙后把大致的功能模块做出来了，但是还有一些细节待处理，例如，iOS状态栏显示不出来。。
- 以为像以前那样子设置[UIApplication sharedApplication].statusBarHidden就可以控制的，然后并没有什么用，点击去一看
```objc
@property(readwrite, nonatomic,getter=isStatusBarHidden) BOOL statusBarHidden NS_DEPRECATED_IOS(2_0, 9_0, "Use -[UIViewController prefersStatusBarHidden]") __TVOS_PROHIBITED;
```
- 然后再搜一下prefersStatusBarHidden,原来前面的人已经使用了这个方法，返回YES,所以状态栏就是隐藏的，于是我想如果在不实现通过prefersStatusBarHidden方法而是只通过设置UIApplication可不可以呢，通过尝试，只是通过方法`[UIApplication sharedApplication].statusBarHidden = YES;`并不能实现隐藏，记得还要设置什么的，本人没有去尝试，有兴趣的朋友可以自己去尝试一下。

#### git
- git删除已经添加的_DS_Sotre
    - find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch
    - 二. 建立.gitignore文件 #增加一行.DS_Store
    - 提交
- 设置全局用户名和邮箱


- 设置
    - 显示隐藏文件`defaults write com.apple.finder AppleShowAllFiles -bool true`


#### RunLoop监听
- 想法
    - 模式切换监听(普通模式，Tracking模式)
    - 状态切换监听(模式监听，唤醒状态)
    - Swift的头文件在实体文件夹中不可见，换言之，要不隐藏了，要不就不不存在(由OC动态生成)，但在同一个类中，里面的解释不一样，所以猜测二不存在，也就是说，Swift头文件本来就是隐藏的，不可见、
    - (Swift)但是为什么点击一个系统框架的某个类，会有转圈
- 额......有时候找东西的时候找着找着就会忘记你想找什么，这样浪费时间，所以无论找东西的时候有什么东西吸引你，你都得忍住，可以收藏起来，好吧，不扯太多，我需要找得是监听在Runloop切换模式(common和Tracking模式)，或者找到状态改变的切入点，这样我就可以在切换不同的模式或者状态，来干一些有意思的事，例如，我在滑动 的时候时候添加一个小狗，在普通模式又把它去掉，所以，我要找得是模式切换的接口
- 网上很多东西一些没什么实际意义的用法，完全没有自己的思想，我是醉了，


- lipo –create /Debug-iphoneos/Someframework.framwork/Someframework
- Debug-iphonesimulator/Someframework.framwork/Someframework
–output Someframework

#### 静态库打包

- lipo -info 查看支持的信息

- lipo –create /Someframework /Someframework –output Someframework

- 静态库打包太大了
    - Also make sure that you set Generate Debug Symbols to NO in your build settings. This can reduce the size of your static library by about 30%.
    - In your target's build settings look for 'Optimization Level'. By switching that to 'Fastest, Smallest -Os' you'll permit the compiler to sacrifice some speed for size.
---
###你的静态库该减肥了
- 我有点震惊，国内的具有极少的有关静态库开发的内容，除了一些简单的如何教你开发，而且是原创还是copy，谁知道呢？细思极恐，不扯那么远了，这里就已切身的爬坑经历提供几个静态库瘦身的方案。
- 公司最近开发完了一套静态库，准备跟CP对接，按部就班地来到了合并静态库的环节，一开始没发现我们的静态库有多么的臃肿，直接到了26M。
- 这是不允许的，想到支付宝的静态库的在8.8M左右，于是想到肯定有减少size的办法。
- 话不多说，直接上解决方案吧，这个方案是解决你的静态库打包太大的问题，亲测可用有效
    - make sure that you set Generate Debug Symbols to NO in your build settings. This can reduce the size of your static library by about 30%.
    - In your target's build settings look for 'Optimization Level'. By switching that to 'Fastest, Smallest -Os' you'll permit the compiler to sacrifice some speed for size.

- 结果我的SDK由原来的26M减少到14M，但是想到一些大公司的大牛肯定有一些更好地一套方案，只是不愿意分享或者我找不到吧，应该还有其他方案的，只是项目比较赶，先这样。

###优化方案
- 数据解析(数据解析太乱了)
- 动画(利用画图技术做一些酷炫的动画)
- 现在全部数据解析都在view层，我要把数据解析全部移到网络层
- 通知乱飞


####@synthesize的作用
- 为什么公司会用到synthesize这个关键字，之前不是很了解这个字，但是由于这个字出现在项目里面，我一定要知道

#### 网络层架构
- manager
- reformer
- 利用协议代替集成的空方法
- AIFURLResponse


#### 项目bug
- 为什么返回数据是php
    - request参数的编码方式

#### 打包步骤
- Generate Debug Symbols to NO
- 'Optimization Level'. By switching that to 'Fastest, Smallest -Os'
- 打印取消

#### UIView的全部方法过一遍
#####touchBegin

 想
��A控件的子控件是B，
