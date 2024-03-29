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



# Multithreading

## Queues
Swift manages the complexity of making multithread code authorable, readable and understandable using **queues**

A queue is just a bunch of _blocks of code_, _lined up_, waiting for a thread to execute them. 

You don't worry about the threads in swift, you are concerned only with queues. 

- **main queue** serves all UI rendering
- **Background queues**: non-UI tasks (including animation)

## Grand Central Dispatch (GCD)
Two fundamental tasks: 
1. getting access to a queue
2. plopping a block of code on a queue

```
let queue = DispatchQueue.main // or DispatchQueue.global(qos:)
queue.async { }  // almost always use async
queue.sync { }
```

!! IMPORTANT: Do not publish things to cause UI to rebuild on background queues. **Anything that modifies UI runs on main thread!!**


## Closure capturing
default capture is **strong capture**. 

A weak capture 
```
closure(...) { [weak id] in 
  id?.present
}
```

# Gestures
- Discreate gestures (onEnded {...})
- Non-Discrete gestures

```
var theGesture: some Gesture {
  DragGesture(...)
    // updating will cause the closure you pass to it to be called when fingers move.
    // $myGestureState: is @GestureState
    // myGestureState is essentially your @GestureState. the only place you can change @GestureState
    // transaction argument has to do with animation, not very often used.
    .updating($myGestureState) { value, myGestureState, transaction in
      myGestureState = compute(value)
    }
    .onEnded { value in 
      /* do something */
    }
}
```

# Property Wrappers
All of property wrappers are `struct`s

```
@Published var emojiArt: EmojiArt = EmojiArt()

// is wirtten as below internally
struct Publised {
  var wrappedValue: EmojiArt
  // projectedValue can be accessed by $emojiArt
  var projectedValue: Publisher<EmojiArt, Never>
}

var _emojiArt: Published = Published(wrappedValue: EmojiArt())
var emojiArt: EmojiArt {
  get { _emojiArt.wrappedValue }
  set { _emojiArt.wrappedValue = newValue }
}
```

- @State
- @StateObject
- @ObservedObject
- @Binding
- @EnvironmentObject
- @Environment

## Binding
> Bindings are always about the source of truth

```
struct MyView: View {
  @State var myString = "Hello"
  var body: some View {
    OtherView(sharedText: $myString)
  }
}

struct OtherView: View {
  @Binding var sharedText: String
  var body: some View {
    Text(sharedText)
    TextField("Shared", text: $sharedText)
  }
}
// OtherView's sharedText is bound to MyView's myString
// Changing sharedText changes myString (and vice versa)
```





