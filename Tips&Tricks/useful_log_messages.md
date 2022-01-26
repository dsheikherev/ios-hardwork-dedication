# Useful log messages

Prints function name and line number in Objective-C:
```
NSLog((@"%s [Line %d] "), __PRETTY_FUNCTION__, __LINE__);
```


Prints function name and line number in Swift:
```
func pretty_function(_ file: String = #file, function: String = #function, line: Int = #line) {
    
    let fileString: NSString = NSString(string: file)
    
    if Thread.isMainThread {
        print("file:\(fileString.lastPathComponent) function:\(function) line:\(line) [M]")
    } else {
        print("file:\(fileString.lastPathComponent) function:\(function) line:\(line) [T]")
    }
}
```