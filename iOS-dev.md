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

