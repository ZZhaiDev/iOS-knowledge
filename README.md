# iOS-knowledge
iOS Domain knowledge

### 1. Thread safe array, Concurrent with .Barrier, Readers–writers problem.
   * [Thread safe code](https://gist.github.com/basememara/afaae5310a6a6b97bdcdbe4c2fdcd0c6)
   * [Concurrency in Swift: Reader Writer Lock](https://medium.com/@dmytro.anokhin/concurrency-in-swift-reader-writer-lock-4f255ae73422)
   * [Dispatch Barriers in Swift 3](https://medium.com/@oyalhi/dispatch-barriers-in-swift-3-6c4a295215d6)
   * [Creating Thread-Safe Arrays in Swift](https://basememara.com/creating-thread-safe-arrays-in-swift/)
   * [Create thread safe array in Swift](https://stackoverflow.com/questions/28191079/create-thread-safe-array-in-swift)
   * [Swift 线程安全数组](https://segmentfault.com/a/1190000012068832)
   
   What if there was a way that you can ensure no writing occurs while reading and no reading occurs while writing using
   concurrency? Well, there is a way to this using `Concurrent Queue with Barrier`. 
   If we take the concurrent code above, and insert a barrier to the write operation,  we ensure that the `writing will occur      after all the reading in the queue is performed and that no reading occurs while writing`.
   * why not using serial queue?
   
   Performance issue. The reads are not optimized because multiple read requests have to wait for each other in a queue. However, reads should be able to happen concurrently, as long as there isn’t a write happening at the same time.
   * Why writing asyc, reading syc?
   
   Notice the async method has the barrier flag set for writes. This means no other blocks may be scheduled from the queue while the async/barrier process runs. We continue to use the sync method for reads, but all readers will run in parallel this time because of the concurrent queue attribute
   ```swift
     private let concurrentQueue = DispatchQueue(label: "concurrentQueue", attributes: .concurrent)
     private var dictionary: [String: Any] = [:]
    
     public func set(_ value: Any?, forKey key: String) {
        // .barrier flag ensures that within the queue all reading is done
        // before the below writing is performed and
        // pending readings start after below writing is performed
        concurrentQueue.async(flags: .barrier) {
            self.dictionary[key] = value
        }
    }

    public func object(forKey key: String) -> Any? {
        var result: Any?
        concurrentQueue.sync {
            result = dictionary[key]
        }
        // returns after concurrentQueue is finished operation
        // beacuse concurrentQueue is run synchronously
        return result
    }
  ```
  
 ### 2. How to avoid blocking UI, what tools we can use?
 * [Avoid blocking UI when doing a lot of operations that are required to be on main thread
](https://stackoverflow.com/questions/11232665/avoid-blocking-ui-when-doing-a-lot-of-operations-that-are-required-to-be-on-main)
* [iOS How to determine what is blocking the UI](https://stackoverflow.com/questions/15928035/ios-how-to-determine-what-is-blocking-the-ui)

 Tools: [Main Thread Checker](https://developer.apple.com/documentation/code_diagnostics/main_thread_checker), System Trace tool, Time Profiler
 
 ### 3. Storyboard vs Pure Code, vs Nib/Xib
 * [iOS User Interfaces: Storyboards vs. NIBs vs. Custom Code](https://www.toptal.com/ios/ios-user-interfaces-storyboards-vs-nibs-vs-custom-code)
  * [Storyboard vs Programmatically in Swift](https://medium.com/@chan.henryk/storyboard-vs-programmatically-in-swift-9a65ff6aaeae)
  ```swift
      The main reason why I refused to use Storyboard and Xib is merge conflict.
      Both code and xib are reuseble.
      The view has a complicated or dynamic layout, best-implemented with code
  ```
  ### 4. Weak, Strong, Automatic Reference Counting, Capturing values, Closures
   * [Capturing Values In Swift Closures](https://marcosantadev.com/capturing-values-swift-closures/)
   * [the swift programming language swift 5.1](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)
   * `A weak reference` is a reference that does not keep a strong hold on the instance it refers to, and so does not stop ARC from disposing of the referenced instance. This behavior prevents the reference from becoming part of a strong reference cycle.
   * whenever you assign a class instance to a property, constant, or variable, that property, constant, or variable makes a `strong reference` to the instance. The reference is called a `“strong” reference` because it keeps a firm hold on that instance, and does not allow it to be deallocated for as long as that strong reference remains.
   * This can happen if two class instances hold a strong reference to each other, such that each instance keeps the other alive. This is known as a `strong reference cycle.`
   
  ### 5. delegate
  * [Implementing delegates in Swift, step by step](https://medium.com/@jamesrochabrun/implementing-delegates-in-swift-step-by-step-d3211cbac3ef)
  * [Delegation in Swift](https://www.swiftbysundell.com/posts/delegation-in-swift)
  * [Quick Guide to Swift Delegates](https://useyourloaf.com/blog/quick-guide-to-swift-delegates/)
  * The core purpose of the delegate pattern is to allow an object to communicate back to its owner in a decoupled way. By not requiring an object to know the concrete type of its owner, we can write code that is much easier to reuse and maintain.
  * you can monitor values and respond when they change.
  * Delegates are a design pattern that allows one object to send messages to another object when a specific event happens.
  * Imagine an object A calls an object B to perform an action. Once the action is complete, object A should know that B has completed the task and take necessary action, this can be achieved with the help of delegates!
  * The problem is that this views are part of class B and have no idea about class A, so we need to find a way to communicate between this two classes, and that’s where delegation shines.
  
  ### 6. Delegate VS Notification
   * [what is the difference between Delegate and Notification?](https://stackoverflow.com/questions/5325226/what-is-the-difference-between-delegate-and-notification)
   * Delegate is passing message from one object to other object. It is like one to one communication while nsnotification is like passing message to multiple objects at the same time. All other objects that have subscribed to that notification or acting observers to that notification can or can’t respond to that event. Notifications are easier but you can get into trouble by using those like bad architecture. Delegates are more frequently used and are used with help of protocols.
   * You can think of delegates like a telephone call. You call up your buddy and specifically want to talk to them. You can say something, and they can respond. You can talk until you hang up the phone. Delegates, in much the same way, create a link between two objects, and you don't need to know what type the delegate will be, it simply has to implement the protocol. On the other hand, NSNotifications are like a radio station. They broadcast their message to whoever is willing to listen. The radio station can't receive feedback from it's listeners (unless it has a telephone, or delegate). The listeners can ignore the message, or they can do something with it. NSNotifications allow you to send a message to any objects, but you won't have a link between them to communicate back and forth. If you need this communication, you should probably implement a delegate. Otherwise, NSNotifications are simpler and easier to use, but may get you into trouble.
   

 ### 7. GCD
 [https://github.com/zijiazhai/GCD](https://github.com/zijiazhai/GCD)
 
  ### 8. What is lazy loading
  Lazy loading means the calculation of the property’s value will not occur until the first time it is needed.
  
  ### 9. Difference between Concurrrent and Serial queues.
  * `Concurrent`: multiple tasks at the same time
  * `Serial`: first come first served, first in first out, ordered
  
  ### 10. What is memory management and ARC
  
* A program needs to allocate and deallocate memory as needed. Swift does that automatically. Swift does not use a garbage collector which is a common tool. ARC is Automatic Reference Counting introduced for Obj-C.
* Everything is automatically managed, but you need to know ARC to avoid memory leaks.

`Retain Cycles?`
* Retain cycles happens when a reference is pointing to another reference so it will never be removed from the heap.
This mostly happens in classes and closures. Closures live in memory, so when you use Self which is a reference you need to make sure that you solve the retain cycle.

`Weak, unowned, strong?`
* Unless you specify otherwise, all Swift properties are strong, which means they will not be removed from RAM until whatever owns them is removed from RAM.
* Weak on the other hand is there when you want to say “I want to be able to reference this variable, but I don’t mind if it goes away, so I don’t want to own it.” This might seem strange: after all, where’s the point in having a reference to a variable that might not be there?
* Unowned means “don’t mind this one, I will make sure it is removed from memory.”

  ### 11. What is MVC & Other Design Patterns You Use?
  
`Model-View-Controller` is a design pattern based on three job categories: model, view or controller.
* `Model`: Models are responsible for storing data and making it available to other objects.
* `View`: Views are the visual elements of an application. What you see on screen.
* `Controllers`: Controllers perform the logic necessary connect your views and models. They process events.
* `Model`: class, struct, properties, relationships
* `View`: Drawing, layout, animation, event detection.
* `Controllers`: One controller per screen, network requests, event handling(taps, swipes), persistence, navigation.

You may use different kinds of design patterns when you are developing your apps, but here are the most common ones.
* `Singleton`: The Singleton design pattern ensures that only one instance exists for a given class
* `Singleton Plus`: you can create another object. Doesn’t force you to use the shared one.
* `Facade`: The Facade design pattern provides a single interface to a complex subsystem. Let’s say you have NetworkManager class where you make HTTP requests and you have JSON response. With using Facade, you can keep your NetworkManager just focused on networking.
* `Decorator`: The Decorator pattern dynamically adds behaviors and responsibilities to an object without modifying its code.
* `Adapter`: An Adapter allows classes with incompatible interfaces to work together. It wraps itself around an object and exposes a standard interface to interact with that object.
* `Observer`: In the Observer pattern, one object notifies other objects of any state changes. Cocoa implements the observer pattern in two ways: `Notifications and Key-Value Observing (KVO)`.

### 12. What is the difference between value types and reference types?
The major difference is that value types are copied when they are passed around, whereas reference types share a single copy of the referenced information.

### 13. What are the main differences between Structures and Classes?
Classes support inheritance, structures don’t. Classes are reference types, structures are value types.

### 14. What is an optional in Swift and what problem do optionals solve?
An optional is used to let a variable of any type represent the lack of value. An optional variable can hold either a value or nil any time.

### 15. What are the various ways to unwrap an optional? How do they rate in terms of safety?
* `forced unwrapping` ! operator — unsafe.
* `implicitly unwrapped` variable declaration — unsafe in many cases.
* `optional binding` — safe.
* `optional chaining` — safe. 
* `nil coalescing operator` — safe. 
* `guard statement` — safe. 
* `optional pattern` — safe

### 16. What is auto-layout
`Auto layout` dynamically calculates the size and position of all the views in your view hierarchy based on constraints placed on those views.

### 17. What is runloop
* A run loop is an abstraction that (among other things) provides a mechanism to handle system input sources (sockets, ports, files, keyboard, mouse, timers, etc).
* Run loops are part of the fundamental infrastructure associated with threads. A run loop is an event processing loop that you use to schedule work and coordinate the receipt of incoming events. The purpose of a run loop is to keep your thread busy when there is work to do and put your thread to sleep when there is none.
* RunLoop的基本作用： 1、保持程序的持续运行 2、处理App中的各种事件（比如触摸事件、定时器事件等） 3、节省CPU资源，提高程序性能：该做事时做事，该休息时休息
* 它内部就是do-while循环，在这个循环内部不断地处理各种任务。（It is a do-while loop inside, constantly processing various tasks inside this loop.）
* 主线程的RunLoop默认已经启动，子线程的RunLoop得手动启动（调用run方法）（The main thread's RunLoop has been started by default, and the child thread's RunLoop has to be started manually.）
* 每条线程都有唯一的一个与之对应的RunLoop对象（Each thread has a unique RunLoop object corresponding to it.）

### 18. cache vs database
* Cache data can be used for any data that needs to persist longer than temporary data, but not as long as a support file. Generally speaking, the application does not require cache data to operate properly, but it can use cache data to improve performance. Examples of cache data include (but are not limited to) database cache files and transient, downloadable content. Note that the system may delete the Caches/ directory to free up disk space, so your app must be able to re-create or download these files as needed.

### 19. Userdefault save data
* Userdefault not encrypted, can be slow when plist is large because have to retrive and save the whole entire file when make change.
``` swift
class dataToSave: NSObject, NSCoding {
    func encode(with aCoder: NSCoder) {
        aCoder.encode(id, forKey: "id")
        aCoder.encode(data, forKey: "data")
    }
    
    required init?(coder aDecoder: NSCoder) {
        self.id = aDecoder.decodeObject(forKey: "id") as? Int
        self.data = aDecoder.decodeObject(forKey: "data") as? [URL]
    }
    
    var id: Int?
    var data: [URL]?
    
    init(id: Int, data: [URL]) {
        self.id = id
        self.data = data
    }
}

if let decoded = UserDefaults.standard.object(forKey: dataBaseKey) as? Data {
            var decodedTeams = NSKeyedUnarchiver.unarchiveObject(with: decoded) as! [dataToSave]
            let obj = dataToSave(id: decodedTeams.last!.id! + 1, data: val)
            decodedTeams.append(obj)
            let encodeData = NSKeyedArchiver.archivedData(withRootObject: decodedTeams)
            UserDefaults.standard.set(encodeData, forKey: dataBaseKey)
        } else {
            var arr = [dataToSave]()
            let obj = dataToSave(id: 1, data: val)
            arr.append(obj)
            let encodeData = NSKeyedArchiver.archivedData(withRootObject: arr)
            UserDefaults.standard.set(encodeData, forKey: dataBaseKey)
        }
```

### 20. Stored properties and extensions: a pure Swift approach
* [https://medium.com/@valv0/computed-properties-and-extensions-a-pure-swift-approach-64733768112c](https://medium.com/@valv0/computed-properties-and-extensions-a-pure-swift-approach-64733768112c)
```swift
  extension UIViewController {
    struct Holder {
        static var _myComputedProperty:Bool = false
    }
    var myComputedProperty:Bool {
        get {
            return Holder._myComputedProperty
        }
        set(newValue) {
            Holder._myComputedProperty = newValue
        }
    }
}
```
  
### 21. Cache data
*  iOS will automatically remove objects from the cache if the device is running low on memory.
```swift
    let cache = NSCache<NSString, ExpensiveObjectClass>()
    let myObject: ExpensiveObjectClass

    if let cachedVersion = cache.object(forKey: "CachedObject") {
        // use the cached version
        myObject = cachedVersion
    } else {
        // create it from scratch then store in the cache
        myObject = ExpensiveObjectClass()
        cache.setObject(myObject, forKey: "CachedObject")
    }
```

### 22. DispatchGroup
```swift
    func requestData(_ finishCallback : @escaping () -> ()) {
        // 1.定义参数
        let parameters = ["limit" : "4", "offset" : "0", "time" : Date.getCurrentTime()]
        
        // 2.创建Group
        let dGroup = DispatchGroup()
        
        // 3.请求第一部分推荐数据
        dGroup.enter()
        NetworkTools.requestData(.get, URLString: "http://capi.douyucdn.cn/api/v1/getbigDataRoom", parameters: ["time" : Date.getCurrentTime()]) { (result) in
            doSomeCodeHere
            // 3.3.离开组
            dGroup.leave()
        }
        
        // 4.请求第二部分颜值数据
        dGroup.enter()
        NetworkTools.requestData(.get, URLString: "http://capi.douyucdn.cn/api/v1/getVerticalRoom", parameters: parameters) { (result) in
            doSomeCodeHere
            // 3.3.离开组
            dGroup.leave()
        }
        
        // 5.请求2-12部分游戏数据
        dGroup.enter()
        // http://capi.douyucdn.cn/api/v1/getHotCate?limit=4&offset=0&time=1474252024
        loadAnchorData(isGroupData: true, URLString: "http://capi.douyucdn.cn/api/v1/getHotCate", parameters: parameters) {
            dGroup.leave()
        }
        
        // 6.所有的数据都请求到,之后进行排序
        dGroup.notify(queue: DispatchQueue.main) { 
            finishCallback()
        }
    }
```
  
  ### 23 Semaphore
  * [A Quick Look at Semaphores in Swift ](https://medium.com/swiftly-swift/a-quick-look-at-semaphores-6b7b85233ddb)
  
  ### 24 如何确保api call是来自app, 确保合法性
  * DeviceCheck API还可以让您验证您收到的token是否来自已下载应用程序的真实Apple设备。
  * The DeviceCheck APIs let you verify that the token you receive comes from an authentic Apple device on which your app has been downloaded.
 

