## <a name="simulate"> Simulate
The simulate method will call the `_process` or `_fixed_process` on a tree of objects.  It takes in the base object, the number of times to call the methods and the delta value to be passed to `_process` or `_fixed_process` (if the object has one).  This will only cause code directly related to the `_process` and `_fixed_process` methods to run.  Signals will be sent, methods will be called but timers, for example, will not fire since the main loop of the game is not actually running.  Creating a test that yields is a better solution for testing such things.

Example
``` python

# --------------------------------
# res://scripts/my_object.gd
# --------------------------------
extends Node2D
  var a_number = 1

  func _ready():
    set_process(true)

  func _process(delta):
    a_number += 1

# --------------------------------
# res://scripts/another_object.gd
# --------------------------------
extends Node2D
  var another_number = 1

  func _ready():
    set_fixed_process(true)

  func _fixed_process(delta):
    another_number += 1

# --------------------------------
# res://test/unit/test_my_object.gd
# --------------------------------

# ...

var MyObject = load('res://scripts/my_object.gd')
var AnotherObject = load('res://scripts/another_object')

# ...

# Given that SomeCoolObj has a _process method that incrments a_number by 1
# each time _process is called, and that the number starts at 0, this test
# should pass
func test_does_something_each_loop():
  var my_obj = MyObject.new()
  add_child(my_obj)
  gut.simulate(my_obj, 20, .1)
  assert_eq(my_obj.a_number, 20, 'Since a_number is incremented in _process, it should be 20 now')

# Let us also assume that AnotherObj acts exactly the same way as
# but has SomeCoolObj but has a _fixed_process method instead of
# _process.  In that case, this test will pass too since all child objects
# have the _process or _fixed_process method called.
func test_does_something_each_loop():
  var my_obj = MyObject.new()
  var other_obj = AnotherObj.new()

  add_child(my_obj)
  my_obj.add_child(other_obj)

  gut.simulate(my_obj, 20, .1)

  assert_eq(my_obj.a_number, 20, 'Since a_number is incremented in _process, \
                                  it should be 20 now')
  assert_eq(other_obj.another_number, 20, 'Since other_obj is a child of my_obj \
                                           and another_number is incremened in \
                                           _fixed_process then it should be 20 now')

```
