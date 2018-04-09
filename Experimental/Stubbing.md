Tests now have the `stub` keyword.  Stubbing works in conjunction with Doubles.  You can stub any doubled class to return values.  Stubs can be layered to address general and specific situations.

# Syntax
* `stub(obj, method)`  This stubs the value for a method.  You must at least call `to_return(...)` on the result in order for a value to be stubbed.  The first paraemter can be a path to a script or a loaded script or an instance of a doubled object.  More on that below
* `to_return(value)` This must be called on the result of `stub` in order to set the value.  For example:
```
stub('res://script.gd', 'some_method').to_return(-1)
```
* `when_passed`  This can optionally be called on the result of `stub` in order to stub a value when specific parameters are passed to the method.  For example:
```
stub('res://script.gd', 'some_method').to_return(-5).when_passed('five')
```

# Example 1

Here is a simple example

Given the `double_this` class:
```
# res://scripts/double_this.gd
extends Node2D

var foo = -1
var bar = 10

func return_seven():
  return 7

func return_hello(param=1):
  return 'hello'

class InnerClass:
  var another_foo = 100
```
Then, from inside your test script you can do the following to alter the `return_seven` method to return `500`
```
func test_something():
  var inst = double('res://scripts/double_this.gd').new()
  stub('res://scripts/double_this.gd', 'return_seven').to_return(500)
  assert_eq(inst.return_seven, 500)
```
The `stub` method is pretty smart about what you pass it.  You can pass it a path, an Object or an instance.  If you pass an instance, it __must__ be an instance of a double.  

When passed an Object or a path, it will stub the value for __all__ instances that do not have explicitly defined stubs.  When you pass it an instance of a doubled class, then the stubbed return will only be set for that instance.
```
var DoubleThis = load('res://scripts/double_this.gd')
var Doubled = double('res://scripts/double_this.gd')

# these two are equivalent
stub('res://scripts/double_this.gd', 'return_seven').to_return(500)
stub(DoubleThis, 'return_seven').to_return(500)

# This will stub the value for the passed in instance ONLY.
# Any other instances will return 500 from the lines above.
var doubled_inst = Doubled.new()
stub(doubled_inst, 'return_seven').to_return('words')
```

# Stubbing based off of parameters
You can stub a method to return a specific value based on what was passed in.
```
const DOUBLE_THIS_PATH = 'res://scripts/double_this.gd'
var Doubled = double(DOUBLE_THI_PATH)
stub(DOUBLE_THIS_PATH, 'return_hello').to_return('world').when_passed(7)
```
The ordering of `when_passed` and `to_return` does not matter.
