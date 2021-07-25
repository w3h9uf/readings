- Difference between `struct` and `class`
  - `struct` 
    - value type, copied when passed or assigned
    - no inheritance
    - free init initialize all vars
    - Mutability must be explicitly stated 
  - `class` 
    - reference type, passed around via pointers. (automatic reference counting)
    - single inheritance
    - free init intializes NO vars
    - always mutable


`private(set)` mean read-only data member in class/struct


`ObservableObject` in ViewModel, `@Published` for watched models, and with `@ObservedObject` on view, we can make the view responsible to gestures


`enum` will be copied around, enum items can take a some data. 

`Optional` is just an enum!!!

```
enum Optional<T> {
  case none
  case some(T)
}
```

```
// safely get optional value
let hello: String? = ...
if let safehello = hello {
  print(safehello)
} else {
}
```

`typealias` for type def 


## Computed Properties
can define `get` and `set` method, `get` is the default one if not specified

## Property Observers
syntactically similar with computed properties but different thing
`willSet` (`newValue`) watches the change of property and invoke the observer function
the same for `didSet` (`oldValue`)


## Protocol
Implementation can be added to a protocol by creating an `extension` to it.

Adding extensions to protocols is key to protocol-oriented programming in swift


## Shape 

### generic functions
```
func fill<S>(_ whatToFillWith: S) -> some View where S: ShapeStyle { ... }
```

## Color

- Color
- UIColor
- CGColor

## Image

- Image (a View)
- UIImage (manipulating images, use Image(uIImage) to display it)





