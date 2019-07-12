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

