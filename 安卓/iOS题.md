- autorelease 和 @autoreleasepool区别

  - autorelease
     1 MRC下autorelease可以延迟对象内存释放，需要对象调用autorelease才会入池，ARC下则不需要调用也可进入。
     2 对象调用autorelease方法进入自动释放池，多个对象调用同一个对象进入同一个池。

     autoreleasepool
     1 在自动释放池被销毁是，会给池里面的对象发送release消息。调用多个autorelease时，会发送对应次数的release消息。
     2 只有在自动释放池栈栈顶的自动释放池才是激活的状态。

    ​

    ​

 		