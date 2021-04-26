# Swift Memory Layout (Swift 5.3)

## Notes about how objects lays out in memory.

### Size, Alignment, Stride

>The __Alignment__ of a type is the maximal alignment of all its’ fields.
>
>The __Stride__ of a type is greater or equal to type's size and divides by alignment with 0 remaining.

<br>Try
```
MemoryLayout<Int>.size
MemoryLayout<Int>.alignment
MemoryLayout<Int>.stride

// or

let myInt: Int = 5
MemoryLayout.size(ofValue: myInt)
MemoryLayout.stride(ofValue: myInt)
MemoryLayout.alignment(ofValue: myInt)
```
to quickly find out size, alignment, stride.

To see byte by byte alignment:
```
var myInt: Int = 42

withUnsafeBytes(of: &myInt) { (pointer: UnsafeRawBufferPointer) in
    for (index, byte) in pointer.enumerated() {
        print("\(index): \(byte)")
    }
}
```

<br>Some results for basic Types:
* __Bool__ : size = 1, alignment = 1, stride = 1
* __Int8__ : size = 1, alignment = 1, stride = 1
* __Int16__, __Float16__ : size = 2, alignment = 2, stride = 2
* __Int32__, __Float__ : size = 4, alignment = 4, stride = 4
* __Int64__, __Int__, __Double__ : size = 8, alignment = 8, stride = 8
* __Float80__ : size = 16, alignment = 16, stride = 16
* __String__, __Character__ : size = 16, alignment = 8, stride = 16

What's wrong with String?

<br>Ok, the interesting part now:
<br>Collections' layout:
* __Array<T>__, __Dictionary<K, V>__ : size = 8, alignment = 8, stride = 8
<br> That's because an Array or Dictionary contains reference which points to heap where elements are located.

Optionals' layout:
* __Int?__, __Double?__ : size = 9, alignment = 8, stride = 16

>Optionals for value types are implemented by adding an extra byte to the end, where 0 means the optional contains a value, and 1 means the optional is nil.
* __Bool?__: size = 1, alignment = 1, stride = 1

>What's wrong with Optional< Bool >? It has no extra bytes. Its values are next:
>* 2 - nil,
>* 1 - true,
>* 0 - false. 

Check this in Xcode memory viewer.
```
var optionalBool: Bool? = nil
withUnsafePointer(to: &optionalBool) { print($0) }
print(optionalBool)
```
Put breakpoint near 'print' command, copy address from console, and then go to *Debug -> Debug Workflow -> View Memory*. Investigate memory this address points to.

* __String?__: size = 16, alignment = 8, stride = 16
<br>What's wrong with Optional< String > ?

* __UIViewController?__: size = 8, alignment = 8, stride = 8

>Optionals of reference types have same 8byte size.
>
>No extra byte.
>
>Reference types are implemented by storing the address when the optional contains a value, and storing zero when the optional contains nil.