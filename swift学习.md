### Swift学习

- 添加成员变量
  
  ``` swift
  public var view: UIView!
  ```


- 如果要使用懒加载
  
  ``` swift
  lazy var animator: UIDynamicAnimator = {
         let tmpAnimator = UIDynamicAnimator(referenceView: self.view)
          return tmpAnimator
      }()
  ```


- 数据类型
  
  - 集合 Array/Set/Dictionary
    
  - 在Swift中，如果你要处理的值不需要改变，那是用常量可以让你的代码更加安全并且更加清晰地表达你的意图
    
  - 元组，让你创建或者传递一组数据，比如作为函数的返回值，你可以用一个元组返回多个值
    
  - let 声明常量，var声明变量
    
  - swift是类型安全的
    
  - double的精度比float精度高，如果swift推算为浮点数的时候，swift总是选择double而不是Float.
    
  - 如果类型没有变过而有定义成var 会警告
    
  - 元组
    
    - 把多个之组合成一个复合值，元组内的值可以使任意类型，并不要求是相同类型。
      
      ``` swift
      let http404Error = (404, "Not Found")
      // http404Error 的类型是 (Int, String)，值是 (404, "Not Found")
      ```
      
    - (404， "Not Found")元组吧一个Int和一个String值组合起来表示HTTP状态码的两部分：一个数字和一个人类可读。这个元组可以被描述为"一个类型(Int , String)的元组"。
      
    - 你可以讲一个元组的内容分解成单独的常量和变量，然后你就可以正常使用它们了：
      
      ``` swift
      let http404Error = (404, "Not Found")
      let (statusCode, statusMessage) = http404Error
      print("The status code is \(statusCode)")
      // 输出 "The status code is 404"
      print("The status message is \(statusMessage)")
      // 输出 "The status message is Not Found"
      print("The status code is \(http404Error.0)")
      // 输出 "The status code is 404"
      print("The status message is \(http404Error.1)")
      // 输出 "The status message is Not Found"
      ```
      
    - 你可以在定义元组的时候给单个元素命名：( ⊙o⊙ )哇！！这就是OC的字典啊
      
      ``` swift
      let http200Status = (statusCode: 200, description: "OK")
      ```
      
      ​
  
- 类型别名
  
  ``` swift
  typealias AudioSample = UInt16
  var maxAmplitudeFound = AudioSample.min
  // maxAmplitudeFound 现在是 0
  ```
  
  ​

#### day01(柯里化 && Selector && protocol)

##### 柯里化

- Swift里可以将方法进行 柯里化(Currying)，这是也就是把多个参数的额方法进行一些变形，使其更加灵活的方法。函数式变成思想贯穿Swift中，而函数的柯里化正是这门语言函数式特点的重要表现。

##### Selector

- 最后需要注意的是，selector 其实是 Objective-C runtime 的概念，如果你的 selector 对应的方法只在 Swift 中可见的话 (也就是说它是一个 Swift 中的 private 方法)，在调用这个 selector 时你会遇到一个 unrecognized selector 错误：
  
  ``` objective-c
  // 错误代码
  private func callMe() {
       //...
  }
  NSTimer.scheduledTimerWithTimeInterval(1, target: self,
              selector:"callMe", userInfo: nil, repeats: true)
    
   // 正确的
    @objc private func callMe() {
      //...
  }
  
  NSTimer.scheduledTimerWithTimeInterval(1, target: self,
               selector:"callMe", userInfo: nil, repeats: true
  ```


- 另外，如果方法的第一个参数有外部变量的话，在通过字符串生成 `Selector` 时还有一个约定，那就是在方法名和第一个外部参数之间加上 `with`：
  
  ``` objective-c
  func aMethod(external paramName: AnyObject!) { ... }
  
  // 想获取对应的获取 Selector，应该这么写：
  let s = Selector("aMethodWithExternal:")
  ```

##### protocol

- Swift的protocol不仅可以被class类型实现，也适用于struct和enum，因为这个原因，我们写给别人用的接口时需要多考虑是否使用mutating来修饰方法，Swift的mutating关键字修饰方法是为了能在改方法中修改struct或是enum的变量所以如果你没在接口方法里写mutating的话，别人如果用struct或者enum来实现这个接口的话，就不能在方法里改变自己的变量。

``` objective-c
protocol Vehicle
{
    var numberOfWheels: Int {get}
    var color: UIColor {get set}

    mutating func changeColor()
}

struct MyCar: Vehicle {
    let numberOfWheels = 4
    var color = UIColor.blueColor()

    mutating func changeColor() {
        color = UIColor.redColor()
    }
}
```

- 在使用 `class` 来实现带有 `mutating` 的方法的接口时，具体实现的前面是不需要加`mutating` 修饰的，因为 `class` 可以随意更改自己的成员变量。所以说在接口里用 `mutating` 修饰方法，对于 `class` 的实现是完全透明，可以当作不存在的。

