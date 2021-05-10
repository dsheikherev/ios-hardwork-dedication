# Swift Attributes
##

* @discardableResult : you can safely ignore function's return value if you don’t need it
* @objcMembers : automatically generate Objective-C thunks for all methods in the class, so you don’t need to mark them individually using @objc
```
@objcMembers class ViewController: UIViewController {
    // code
}
```