# defer 
##

* __defer__ block is executed when the current scope exits (_any scope!_). Also you can have multiple defer blocks within one scope!
And they will be executed in reverse order or LIFO order. 

For example:
```
func test() {
    print("A")
    
    defer {
        print("G")
    }
    
    if true {
        defer {
            print("D")
        }
        
        defer {
            print("C")
        }
        
        print("B")
    }
    
    print("E")
    
    defer {
        print("F")
    }
}

test()
```
The result here will be: __A B C D E F G__