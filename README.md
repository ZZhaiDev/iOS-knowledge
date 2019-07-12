# iOS-knowledge
iOS Domain knowledge

### 1. Thread safe array, Concurrent with .Barrier, Readers–writers problem.
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
  
 

