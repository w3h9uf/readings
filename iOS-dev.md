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

