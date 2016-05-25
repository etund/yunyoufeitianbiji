## React Native

#### React

- 基于React进行开发时所有的DOM构造都是通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，然后React将当前整个DOM树和上一次的DOM树进行对比，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新

- JavaScript表达式
  - 属性表达式
    - 要使用JS表达式作为属性值，只需把这个表达式用一堆大括号{}包起来

  - 子节点表达式
    - JS表达式可以用于描述子节点
      ```javascript
      // 输入 (JSX):
      var content = <Container>{window.isLoggedIn ? <Nav /> : <Login />}</Container>;
      // 输出 (JS):
      var content = React.createElement(
        Container,
        null,
        window.isLoggedIn ? React.createElement(Nav) : React.createElement(Login)
      );
      ```

      ​

  - 注释
    - JSX里添加注释很容易，只需要在一个标签的子节点内(非最外层)小心地用 `{}` 包围要注释的部分。

  - JSX(javaScriptExtension) 语法是可选的，但是我们发现 JSX 语句比纯 JavaScript 用起来更容易。

  - 我们通过JavaScript对象传递一些方法到React.createClass()创建一个新的React组件。其中最重要的方法是render，该方法返回一个React组件树，这棵树最终将会渲染成HTML。

  - 我们渲染的评论内容在浏览器里面看起来像这样：“``This is``another`` comment``”。我们希望这些标签能够真正地渲染成 HTML 。那是 React 在保护你免受 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)

  ```javascript
  var Comment = React.createClass({
    rawMarkup: function() {
      var rawMarkup = marked(this.props.children.toString(), {sanitize: true});
      return { __html: rawMarkup };
    },

    render: function() {
      return (
        <div className="comment">
          <h2 className="commentAuthor">
            {this.props.author}
          </h2>
          <span dangerouslySetInnerHTML={this.rawMarkup()} />
        </div>
      );
    }
  });
  ```

  - 这是一个特殊的 API，故意让插入原始的 HTML 变得困难，但是对于 marked ，我们将利用这个后门。
  - 使用这个功能，你的代码就要依赖于 marked 的安全性。在本场景中，我们传入`sanitize: true` ，告诉 marked 转义掉评论文本中的 HTML 标签而不是直接原封不动地返回这些标签。

- 在同一个组件里面的不同方法之间不需要逗号来隔开。

- 样式里面要用逗号隔开。

- 布局

- ```
  前辈教导我们，掌握一门新技术的最快方法是练习
  ```

- 后端渲染，类似于

- 常用样式

  - flex

- 操作

  - 开始项目


  ```code
  $ npm install -g react-native-cli
  $ react-native init AwesomeProject
  ```

  ​

  - 运行安卓应用


  ```code
  $ cd AwesomeProject
  $ react-native run-android
  ```

  ​


#### 布局


- flex的元素如果不设置宽度，都会百分之败的占满父容器。

- 居中

  ```javascript
  <Text style = {[styles.text, styles.header]}>
            水平居中...
          </Text>
          <View style = {{height: 100, backgroundColor: '#333333', alignItems:'center'}}>
            <View style = {{backgroundColor:'#fefefe', width:30, height: 30, borderRadius:15}} />
          </View>

          <Text style = {[styles.text, styles.header]}>
            垂直居中...
          </Text>
          <View style = {{height: 100, backgroundColor: '#333333', justifyContent:'center'}}>
              <View style = {{backgroundColor:'#fefefe', width:30, height: 30, borderRadius:15}} />
          </View>

          <Text style = {[styles.text, styles.header]}>
            水平垂直居中...
          </Text>
          <View style = {{height:100, backgroundColor:'#333333', alignItems:'center', justifyContent:'center'}}>
            <View style = {{backgroundColor:'#fefefe', width:30, height: 30, borderRadius:15}} />
          </View>
  ```


- react 宽度基于`pt`为单位， 可以通过`Dimensions` 来获取宽高，`PixelRatio` 获取密度。

- 基于flex的布局

  1. view默认宽度为100%
  2. 水平居中用`alignItems`, 垂直居中用`justifyContent`
  3. 基于flex能够实现现有的网格系统需求，且网格能够各种嵌套无bug

- 图片布局

  1. 通过`Image.resizeMode`来适配图片布局，包括`contain`, `cover`, `stretch`
  2. 默认不设置模式等于cover模式
  3. contain模式自适应宽高，给出高度值即可
  4. cover铺满容器，但是会做截取
  5. stretch铺满容器，拉伸

- 定位

  1. 定位相对于父元素，父元素不用设置position也行
  2. padding 设置在Text元素上的时候会存在bug。所有padding变成了marginBottom

- 文本元素

  1. 文字必须放在Text元素里边
  2. Text元素可以相互嵌套，且存在样式继承关系
  3. `numberOfLines` 需要放在最外层的Text元素上，且虽然截取了文字但是还是会占用空间

  ​

  ​

#### 事件




- 线程
- 网络


####组件的详细说明和生命周期

- render
  - `render()` 方法是必须的。
  - 当调用的时候，会检测 `this.props` 和 `this.state`，返回一个单子级组件。该子级组件可以是虚拟的本地 DOM 组件（比如 `` 或者 `React.DOM.div()`），也可以是自定义的复合组件。
  - 你也可以返回 `null` 或者 `false` 来表明不需要渲染任何东西
  - `render()` 函数应该是*纯粹的*，也就是说该函数不修改组件 state，每次调用都返回相同的结果，不读写 DOM 信息，也不和浏览器交互（例如通过使用 `setTimeout`）。


- getInitialState

  ```javascript
  object getInitialState()
  ```

  - 在组件挂载之前调用一次。返回值将会作为 `this.state` 的初始值，在组件的生命周期中仅执行一次，用于设置组件的初始化state。

- getDefaultProps

  ```javascript
  object getDefaultProps()
  ```

  - 在组件类创建的时候调用一次，然后返回值被缓存下来。

- propTypes

  ```javascript
  object propTypes
  ```

  - `propTypes` 对象允许验证传入到组件的 props。

- mixins

  ```javascript
  array mixins
  ```

  - `mixin` 数组允许使用混合来在多个组件之间共享行为。更多关于混合的信息，参考[可重用的组件](http://reactjs.cn/react/docs/reusable-components.html)。

- statics

  ```java
  object statics
  ```

  - statics对象允许你定义静态的方法，这些静态方法可以在组件上调用

    ```javascript
    var MyComponent = React.createClass({
      statics: {
        customMethod: function(foo) {
          return foo === 'bar';
        }
      },
      render: function() {
      }
    });

    MyComponent.customMethod('bar');  // true
    ```

  - 在这个块儿里面定义的方法都是静态的，意味着你可以在任何组件实例创建之前调用它们，这些方法不能获取组件的props和state。如果你想在静态方法中检查props的值，在调用处吧props作为参数传入静态方法

- displayName

  - `displayName` 字符串用于输出调试信息。JSX 自动设置该值

####生命周期方法

- componentMount（挂载）

```javascript
componentWillMount()	
```

- - 服务器端和客户端都只调用一次，在初始化渲染执行之前立刻调用。如果在这个方法内调用`setState`，`render()` 将会感知到更新后的 state，将会执行仅一次，尽管 state 改变了。
- componentDidMount(挂载)
  - ​

####jsx的延展性

- 如果你不知道要设置哪些 Props，那么现在最好不要设置它，这样是反模式，因为 React 不能帮你检查属性类型（propTypes）



- ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
- HTML 语言直接写在 JavaScript 语言之中，不加任何引号，这就是 [JSX 的语法](http://facebook.github.io/react/docs/displaying-data.html#jsx-syntax)，它允许 HTML 与 JavaScript 的混写
- 遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{`开头）(类似OGNL语句)，就用 JavaScript 规则解析
- 所有组件类都必须有自己的 `render` 方法，用于输出组件。

#### iOS（Day 01）

- React Native 结合了 Web 应用和 Native 应用的优势，可以使用 JavaScript 来开发 iOS 和 Android 原生应用。在 JavaScript 中用 React 抽象操作系统原生的 UI 组件，代替 DOM 元素来渲染等。

- 样式

  - 声明样式

    - ``` javascript
      var styles = StyleSheet.create({
        base: {
          width: 38,
          height: 38,
        },
        background: {
          backgroundColor: '#222222',
        },
        active: {
          borderWidth: 2,
          borderColor: '#00ff00',
        },
      }); 
      ```

    - `StyleSheet.create` 的创建是可选的，但提供了一些关键优势。它通过将它们转换为引用一个内部表的纯数字，来确保值是**不可变的**和**不透明的**。通过将它放在文件的最后，也确保了它们为应用程序只创建一次，而不是每一个 render 都创建。

  - 使用样式

    - 所有的核心组件接受样式属性，也接受一些列的样式，也可以在render中创建样式对象

  - 样式传递


- 手势应答系统(iOS的响应者链条)

  - 最佳实践

    - 用户在 web 应用程序与本机的可用性上可以感觉到巨大的差异，并且这是最大的原因之一，每一个动作都应该有以下属性：
      - 反馈/高亮——显示给用户是什么正在处理他们的触摸，以及当他们释放手势时，会发生什么
      - 撤销的能力——当做一个动作时，用户应该能够在触摸过程中通过移动手指中止该动作。


    - 应答系统在使用时可能是复杂的。所以我们为应该“可以轻击的”东西提供了一个抽象的 `Touchable` 实现。在网络中任何你会用到按钮或链接的地方使用`TouchableHighlight`。

- 应答器的生命周期

  - 询问视图是否想成为应答器
  - 如果视图返回 true 并且想成为应答器，那么下述的一种情况就会发生
  - 如果视图正在响应，那么可以调用以下处理程序：

- 捕捉ShouldSet处理程序

  - 在冒泡模式，即最深的节点最先被调用，的情况下，`onStartShouldSetResponder` 和 `onMoveShouldSetResponder`被调用。这意味着，当多个视图为 `* ShouldSetResponder` 处理程序返回 true 时，最深的组件会成为应答器。在大多数情况下，这是可取的，因为它确保了所有控件和按钮是可用的。(这就是模仿响应者链条啊)
  - 然而，有时父组件会想要确保它成为应答器。这可以通过使用捕获阶段进行处理。在应答系统从最深的组件冒泡时，它将进行一个捕获阶段，引发 `* ShouldSetResponderCapture`。所以如果一个父视图要防止子视图在触摸开始时成为应答器，它应该有一个 `onStartShouldSetResponderCapture` 处理程序，返回 true。（可想而知它并没有用户交互设置为no的一说）


- Navtive模块

  - js如何访问原生OC

  ``` objective-c
    // CalendarManager.h            // OC
      #import "RCTBridgeModule.h"
      #import "RCTLog.h"
      @interface CalendarManager : NSObject <RCTBridgeModule>
      @endxxxxxxxxxx
  ```






     // CalendarManager.h
      #import "RCTBridgeModule.h"
      #import "RCTLog.h"
      @interface CalendarManager : NSObject <RCTBridgeModule>
      @end
  ```

  ``` javascript
  // JS
  var CalendarManager = require('NativeModules').CalendarManager;
      CalendarManager.addEventWithName('Birthday Party', '4 Privet Drive, Surrey');
  ```

- 导出的方法名称是从OC选择器的第一部分中生成的，也可以生成一个非惯用的JS名称，可以通过RCT_EXPORT提供一个可选参数更改名字，如RCT_EXPORT(addEvent)

- 方法返回类型应该是`void`的，由于。RN桥是异步的，所以JS传递结果的唯一方法是使用回调或emitting事件

  `React Ntive 没有为这些结构中值的类型提供任何担保。你的 native 模块可能期望一个字符串数组，但如果 JavaScript 调用你的包含数字和字符串数组的方法，你会得到带有 `NSNumber` 和 `NSString` 的 `NSArray`。检查数组/映射值类型是开发人员的责任 (助手方法见 `RCTConvert`)。`

- 回调

  - Native 模块还支持一种特殊的参数——回调。在大多数情况下它是用来向 JavaScript 提供函数调用结果的。
  - `RCTResponseSenderBlock` 只接受一个参数——参数的数组传递给 JavaScript 的回调。
  - Native 模块应该只调用它的回调一次。然而，它可以将回调作为 ivar 存储并稍后调用回调。这种模式通常用于包装需要委托的 iOS 的 APIs。请看 `RCTAlertManager`。如果你想向 JavaScript 传递 error ——如对象，使用 `RCTUtils.h` 的 `RCTMakeError`。

  ##### 实现native模块

- 如果实现比较耗时的操作，应当用多线程来实现

  ##### 导出常量

- Native 模块可以在运行时向 JavaScript 导出立即可用的常量。导出一些初始数据是有用的，否则这些初始数据需要往返的桥梁。(就是需要回调桥梁)
- 所以只有在初始化的时候常量才能被导出，所以如果在运行时改变constantsToExport的值，并不会影响到JS环境，所以如果是一些耗时的操作(网络请求)，就不行了

##### 发送事件到JS

- Native 模块可以在不被直接调用的情况下向 JavaScript 发送事件信号。最简单的方法是使用 `eventDispatcher`：(事件分派器)

  ``` objective-c
   #import "RCTBridge.h"
      #import "RCTEventDispatcher.h"
      @implementation CalendarManager
      @synthesize bridge = _bridge;
      - (void)calendarEventReminderReceived:(NSNotification *)notification
      {
          NSString *eventName = notification.userInfo[@"name"];
          [self.bridge.eventDispatcher sendAppEventWithName:@"EventReminder"
                                                 body:@{@"name": eventName}];
      }
      @end
  ```

  ​

- js可以订阅这些事件

  ``` javascript
  var subscription = DeviceEventEmitter.addListener(
    'EventReminder',
    (reminder) => console.log(reminder.name)
  );
  ...
  // Don't forget to unsubscribe
  subscription.remove();
  ```

#### Navtive UI组件(iOS)

- Native 视图是通过 `RCTViewManager` 的子类创建和操做的。这些子类的功能与视图控制器很相似，但本质上它们是单件模式——桥只为每一个子类创建一个实例。它们将 native 视图提供给 `RCTUIManager`，它会传回到 native 视图来设置和更新的必要的视图属性。`RCTViewManager` 通常也是视图的代表，通过桥将事件发送回 JavaScript。



` **一个限制**: React 组件只能渲染单个根节点。如果你想要返回多个节点，它们*必须*被包含在同一个节点里。`







#### 坑坑坑

- React cannot find entry file in any of the roots？
  - Check for the node server running in one of the bash terminal, this was probably kicked by previous ReactNative XCode project you launched earlier. Stop that process and run XCode project again, this should fix the problem.

- could not connect to development server
  -  把AppDelegate里面的的  jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/Examples/Movies/MoviesApp.ios.bundle?platform=ios&dev=true"];里面的localhost换成本机的ip地址

- Error: Cannot find module 'babel-polyfill'
  - sudo npm install -g babel-polyfill

- Adjacent JSX elements must be wrapped in an enclosing tag

  - ```javascript
     //WRONG!
    return (  
               <Comp1 />
               <Comp2 />
           )xxxxxxxxxx 
    ```

    instead:

    ```javascript
    //Correct
    return (
               <div>
                 <Comp1 />
                 <Comp2 />
               </div>
           )
    ```



- Expected a component class, got div. Each component name should start with an uppercase letter.

  - 如果是使用


```javascript
  class AwesomeProject extends Component {}//这样就可以在外层包裹div,如果包裹了div并且没有使用这个开头会包以上错误
```

- 可以替换为


```javascript
  var AwesomeProject = React.createClass({});//在外面不可以包裹div,但是可以包裹View
```

- Unexpected token

  - 语法错误
- Unable to resolve module image!book from /Users/yunyoufeitian/Documents/ReactJS/RN/HelloWorld/index.ios.js: Unable to find this module in its module map or any of the node_modules directories under /Users/node_modules/image!book and its parent directories
  - https://github.com/facebook/react-native/issues/4968
  - Delete the `node_modules` folder - `rm -rf node_modules && npm install`
  - Reset packager cache - `rm -fr $TMPDIR/react-*` or `node_modules/react-native/packager/packager.sh --reset-cache`
  - Clear watchman watches - `watchman watch-del-all`
  - Recreate the project from scratch






- Application HelloWorld has not been registered. This is either due to a require() error during initialization or failure to call AppRegistry.registerComponent.

  - 这样的问题，IOS项目请注意AppDelegate.m中的RCTRootView 初始化的时候的moduleName 

  ​


- 安卓步骤
  - buildTools  23.0.1
  - js文件输出
  - java文件调用
  - ​


#### 参考文章

[一个完整的Flexbox指南](http://www.w3cplus.com/css3/a-guide-to-flexbox.html)

