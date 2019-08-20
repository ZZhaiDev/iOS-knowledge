# iOS-knowledge
iOS Domain knowledge

### 1. Thread safe array, Concurrent with .Barrier, Readers–writers problem.
   * [Concurrency in Swift: Reader Writer Lock](https://medium.com/@dmytro.anokhin/concurrency-in-swift-reader-writer-lock-4f255ae73422)
   * [Dispatch Barriers in Swift 3](https://medium.com/@oyalhi/dispatch-barriers-in-swift-3-6c4a295215d6)
   * [Creating Thread-Safe Arrays in Swift](https://basememara.com/creating-thread-safe-arrays-in-swift/)
   * [Create thread safe array in Swift](https://stackoverflow.com/questions/28191079/create-thread-safe-array-in-swift)
   * [Swift 线程安全数组](https://bignerdcoding.com/archives/58.html)
   
   What if there was a way that you can ensure no writing occurs while reading and no reading occurs while writing using
   concurrency? Well, there is a way to this using `Concurrent Queue with Barrier`. 
   If we take the concurrent code above, and insert a barrier to the write operation,  we ensure that the `writing will occur      after all the reading in the queue is performed and that no reading occurs while writing`.
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
   

 ### 2. GCD
 [https://github.com/zijiazhai/GCD](https://github.com/zijiazhai/GCD)
  
  
 

