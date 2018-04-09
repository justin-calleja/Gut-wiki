Doubles can be created using the `double` method.  This will return an object that can be instanced via `new` or `instance` depending on the source object.  The doubled object will have all the methods defined in the source object but the implementation will be empty.  The doubled class inherits from the source object so it will have all the same variables and Inner Classes defined in the source object.

As of now, only methods not defined in a built in class have their implementation changed.  For example given:

```
# res://scripts/double_this.gd
extends Node2D

var foo = -1
var bar = 10

func return_seven():
  return 7

func return_hello():
  return 'hello'

class InnerClass:
  var another_foo = 100
```
When doubled in a test:
```
func test_doubling():
  var DoubledObj = double('res://scripts/double_this.gd')
  var doubled_inst = DoubledObj.new()
```
Then the methods `return_seven`, `return_hello` will be altered for `doubled_inst`.  The Inner Class `InnerClass` would exist but any methods would not be altered.  The member variables `foo` and `bar` will also exist in `doubled_inst` and they will have the same values as the source class.

Currently you __cannot__ double Inner Classes but should be implemented soon.
