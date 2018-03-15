These methods should be used in tests to make assertions.  These methods are available to anything that inherits from the Test class (`extends "res://addons/gut/test.gd"`).  All sample code listed for the methods can be found here in [test_readme_examples.gd](test/samples/test_readme_examples.gd)
#### pending(text="")
flag a test as pending, the optional message is printed in the GUI
``` python
pending('This test is not implemented yet')
pending()
```
#### <a name="assert_eq"> assert_eq(got, expected, text="")
assert got == expected and prints optional text
``` python
var one = 1
var node1 = Node.new()
var node2 = node1

assert_eq(one, 1, 'one should equal one') # PASS
assert_eq('racecar', 'racecar') # PASS
assert_eq(node2, node1) # PASS

gut.p('-- failing --')
assert_eq(1, 2) # FAIL
assert_eq('hello', 'world') # FAIL
assert_eq(self, node1) # FAIL
```
#### <a name="assert_ne"> assert_ne(got, not_expected, text="")
asserts got != expected and prints optional text
``` python
var two = 2
var node1 = Node.new()

gut.p('-- passing --')
assert_ne(two, 1, 'Two should not equal one.')  # PASS
assert_ne('hello', 'world') # PASS
assert_ne(self, node1) # PASS

gut.p('-- failing --')
assert_ne(two, 2) # FAIL
assert_ne('one', 'one') # FAIL
assert_ne('2', 2) # FAIL
```
#### <a name="assert_gt"> assert_gt(got, expected, text="")
assserts got > expected
``` python
var bigger = 5
var smaller = 0

gut.p('-- passing --')
assert_gt(bigger, smaller, 'Bigger should be greater than smaller') # PASS
assert_gt('b', 'a') # PASS
assert_gt('a', 'A') # PASS
assert_gt(1.1, 1) # PASS

gut.p('-- failing --')
assert_gt('a', 'a') # FAIL
assert_gt(1.0, 1) # FAIL
assert_gt(smaller, bigger) # FAIL
```
#### <a name="assert_lt"> assert_lt(got, expected, text="")
asserts got < expected
``` python
var bigger = 5
var smaller = 0
gut.p('-- passing --')
assert_lt(smaller, bigger, 'Smaller should be less than bigger') # PASS
assert_lt('a', 'b') # PASS

gut.p('-- failing --')
assert_lt('z', 'x') # FAIL
assert_lt(-5, -5) # FAIL
```
#### <a name="assert_true"> assert_true(got, text="")
asserts got == true
``` python
gut.p('-- passing --')
assert_true(true, 'True should be true') # PASS
assert_true(5 == 5, 'That expressions should be true') # PASS

gut.p('-- failing --')
assert_true(false) # FAIL
assert_true('a' == 'b') # FAIL
```
#### <a name="assert_false"> assert_false(got, text="")
asserts got == false
``` python
gut.p('-- passing --')
assert_false(false, 'False is false') # PASS
assert_false(1 == 2) # PASS
assert_false('a' == 'z') # PASS
assert_false(self.has_user_signal('nope')) # PASS

gut.p('-- failing --')
assert_false(true) # FAIL
assert_false('ABC' == 'ABC') # FAIL
```
#### <a name="assert_between"> assert_between(got, expect_low, expect_high, text="")
asserts got > expect_low and <= expect_high
``` python
gut.p('-- passing --')
assert_between(5, 0, 10, 'Five should be between 0 and 10') # PASS
assert_between(10, 0, 10) # PASS
assert_between(0, 0, 10) # PASS
assert_between(2.25, 2, 4.0) # PASS

gut.p('-- failing --')
assert_between('a', 'b', 'c') # FAIL
assert_between(1, 5, 10) # FAIL
```
#### <a name="assert_has"> assert_has(obj, element, text='')
Asserts that the object passed in "has" the element.  This works with any object that has a `has` method.
``` python
var an_array = [1, 2, 3, 'four', 'five']
var a_hash = { 'one':1, 'two':2, '3':'three'}

gut.p('-- passing --')
assert_has(an_array, 'four') # PASS
assert_has(an_array, 2) # PASS
# the hash's has method checks indexes not values
assert_has(a_hash, 'one') # PASS
assert_has(a_hash, '3') # PASS

gut.p('-- failing --')
assert_has(an_array, 5) # FAIL
assert_has(an_array, self) # FAIL
assert_has(a_hash, 3) # FAIL
assert_has(a_hash, 'three') # FAIL
```
#### <a name="assert_does_not_have"> assert_does_not_have(obj, element, text='')
The inverse of `assert_has`
``` python
var an_array = [1, 2, 3, 'four', 'five']
var a_hash = { 'one':1, 'two':2, '3':'three'}

gut.p('-- passing --')
assert_does_not_have(an_array, 5) # PASS
assert_does_not_have(an_array, self) # PASS
assert_does_not_have(a_hash, 3) # PASS
assert_does_not_have(a_hash, 'three') # PASS

gut.p('-- failing --')
assert_does_not_have(an_array, 'four') # FAIL
assert_does_not_have(an_array, 2) # FAIL
# the hash's has method checkes indexes not values
assert_does_not_have(a_hash, 'one') # FAIL
assert_does_not_have(a_hash, '3') # FAIL
```

#### <a name="assert_has_signal"> assert_has_signal(object, signal_name)
Asserts the passed in object has a signal with the specified name.  It should be noted that all the asserts that verfy a signal was/wasn't emitted will first check that the object has the signal being asserted against.  If it does not, a specific failure message will be given.  This means you can usually skip the step of specifically verifying that the object has a signal and move on to making sure it emits the signal correctly.
``` python
class SignalObject:
	func _init():
		add_user_signal('some_signal')
		add_user_signal('other_signal')

func test_assert_has_signal():
	var obj = SignalObject.new()

	gut.p('-- passing --')
	assert_has_signal(obj, 'some_signal')
	assert_has_signal(obj, 'other_signal')

	gut.p('-- failing --')
	assert_has_signal(obj, 'not_a real SIGNAL')
	assert_has_signal(obj, 'yea, this one doesnt exist either')
	# Fails because the signal is not a user signal.  Node2D does have the
	# specified signal but it can't be checked this way.  It could be watched
	# and asserted that it fired though.
	assert_has_signal(Node2D.new(), 'exit_tree')

```
#### <a name="watch_signals"> watch_signals(object)
This must be called in order to make assertions based on signals being emitted.  __Right now, this only supports signals that are emitted with 9 or less parameters.__  This can be extended but nine seemed like enough for now.  The Godot documentation suggests that the limit is four but in my testing I found you can pass more.

This must be called in each test in which you want to make signal based assertions in.  You can call it multiple times with different objects.   You should not call it multiple times with the same object in the same test.  The objects that are watched are cleared after each test (specifically right before `teardown` is called).  Under the covers, Gut will connect to all the signals an object has and it will track each time they fire.  You can then use the following asserts and methods to verify things are acting correct.

#### <a name=assert_signal_emitted> assert_signal_emitted(object, signal_name)
Assert that the specified object emitted the named signal.  You must call `watch_signals` and pass it the object that you are making assertions about.  This will fail if the object is not being watched or if the object does not have the specified signal.  Since this will fail if the signal does not exist, you can often skip using `assert_has_signal`.
``` python
class SignalObject:
	func _init():
		add_user_signal('some_signal')
		add_user_signal('other_signal')

func test_assert_signal_emitted():
	var obj = SignalObject.new()

	watch_signals(obj)
	obj.emit_signal('some_signal')

	gut.p('-- passing --')
	assert_signal_emitted(obj, 'some_signal')

	gut.p('-- failing --')
	# Fails with specific message that the object does not have the signal
	assert_signal_emitted(obj, 'signal_does_not_exist')
	# Fails because the object passed is not being watched
	assert_signal_emitted(SignalObject.new(), 'some_signal')
	# Fails because the signal was not emitted
	assert_signal_emitted(obj, 'other_signal')
```
#### <a name="assert_signal_not_emitted"> assert_signal_not_emitted(object, signal_name)
This works opposite of `assert_signal_emitted`.  This will fail if the object is not being watched or if the object does not have the signal.
``` python
class SignalObject:
	func _init():
		add_user_signal('some_signal')
		add_user_signal('other_signal')

func test_assert_signal_not_emitted():
	var obj = SignalObject.new()

	watch_signals(obj)
	obj.emit_signal('some_signal')

	gut.p('-- passing --')
	assert_signal_not_emitted(obj, 'other_signal')

	gut.p('-- failing --')
	# Fails with specific message that the object does not have the signal
	assert_signal_not_emitted(obj, 'signal_does_not_exist')
	# Fails because the object passed is not being watched
	assert_signal_not_emitted(SignalObject.new(), 'some_signal')
	# Fails because the signal was emitted
	assert_signal_not_emitted(obj, 'some_signal')
```
#### <a name="assert_signal_emitted_with_parameters"> assert_signal_emitted_with_parameters(object, signal_name, parameters, index=-1)
Asserts that a signal was fired with the specified parameters.  The expected parameters should be passed in as an array.  An optional index can be passed when a signal has fired more than once.  The default is to retrieve the most recent emission of the signal.

This will fail with specific messages if the object is not being watched or the object does not have the specified signal
``` python
class SignalObject:
	func _init():
		add_user_signal('some_signal')
		add_user_signal('other_signal')

func test_assert_signal_emitted_with_parameters():
	var obj = SignalObject.new()

	watch_signals(obj)
	# emit the signal 3 times to illustrate how the index works in
	# assert_signal_emitted_with_parameters
	obj.emit_signal('some_signal', 1, 2, 3)
	obj.emit_signal('some_signal', 'a', 'b', 'c')
	obj.emit_signal('some_signal', 'one', 'two', 'three')

	gut.p('-- passing --')
	# Passes b/c the default parameters to check are the last emission of
	# the signal
	assert_signal_emitted_with_parameters(obj, 'some_signal', ['one', 'two', 'three'])
	# Passes because the parameters match the specified emission based on index.
	assert_signal_emitted_with_parameters(obj, 'some_signal', [1, 2, 3], 0)

	gut.p('-- failing --')
	# Fails with specific message that the object does not have the signal
	assert_signal_emitted_with_parameters(obj, 'signal_does_not_exist', [])
	# Fails because the object passed is not being watched
	assert_signal_emitted_with_parameters(SignalObject.new(), 'some_signal', [])
	# Fails because parameters do not match latest emission
	assert_signal_emitted_with_parameters(obj, 'some_signal', [1, 2, 3])
	# Fails because the parameters for the specified index do not match
	assert_signal_emitted_with_parameters(obj, 'some_signal', [1, 2, 3], 1)
```
#### <a name="assert_signal_emit_count"> assert_signal_emit_count(object, signal_name)
Asserts that a signal fired a specific number of times.

``` python
class SignalObject:
	func _init():
		add_user_signal('some_signal')
		add_user_signal('other_signal')

func test_assert_signal_emit_count():
	var obj_a = SignalObject.new()
	var obj_b = SignalObject.new()

	watch_signals(obj_a)
	watch_signals(obj_b)
	obj_a.emit_signal('some_signal')
	obj_a.emit_signal('some_signal')

	obj_b.emit_signal('some_signal')
	obj_b.emit_signal('other_signal')

	gut.p('-- passing --')
	assert_signal_emit_count(obj_a, 'some_signal', 2)
	assert_signal_emit_count(obj_a, 'other_signal', 0)

	assert_signal_emit_count(obj_b, 'other_signal', 1)

	gut.p('-- failing --')
	# Fails with specific message that the object does not have the signal
	assert_signal_emit_count(obj_a, 'signal_does_not_exist', 99)
	# Fails because the object passed is not being watched
	assert_signal_emit_count(SignalObject.new(), 'some_signal', 99)
	# The following fail for obvious reasons
	assert_signal_emit_count(obj_a, 'some_signal', 0)
	assert_signal_emit_count(obj_b, 'other_signal', 283)
```
#### <a name="get_signal_emit_count"> get_signal_emit_count(object, signal_name)
This will return the number of times a signal was fired.  This gives you the freedom to make more complicated assertions if the spirit moves you.  This will return -1 if the signal was not fired or the object was not being watched, or if the object does not have the signal.

#### <a name="get_signal_parameters"> get_signal_parameters(object, signal_name, index=-1)
If you need to inspect the parameters in order to make more complicate assertions, then this will give you access to the parameters of any watched signal.  This works the same way that `assert_signal_emitted_with_parameters` does.  It takes an object, signal name, and an optional index.  If the index is not specified then the parameters from the most recent emission will be returned.  If the object is not being watched, the signal was not fired, or the object does not have the signal then `null` will be returned.
``` python
class SignalObject:
	func _init():
		add_user_signal('some_signal')
		add_user_signal('other_signal')

func test_get_signal_parameters():
	var obj = SignalObject.new()
	watch_signals(obj)
	obj.emit_signal('some_signal', 1, 2, 3)
	obj.emit_signal('some_signal', 'a', 'b', 'c')

	gut.p('-- passing --')
	# passes because get_signal_parameters returns the most recent emission
	# by default
	assert_eq(get_signal_parameters(obj, 'some_signal'), ['a', 'b', 'c'])
	assert_eq(get_signal_parameters(obj, 'some_signal', 0), [1, 2, 3])
	# if the signal was not fired null is returned
	assert_eq(get_signal_parameters(obj, 'other_signal'), null)
	# if the signal does not exist or isn't being watched null is returned
	assert_eq(get_signal_parameters(obj, 'signal_dne'), null)

	gut.p('-- failing --')
	assert_eq(get_signal_parameters(obj, 'some_signal'), [1, 2, 3])
	assert_eq(get_signal_parameters(obj, 'some_signal', 0), ['a', 'b', 'c'])
```
#### <a name="assert_file_exists"> assert_file_exists(file_path)
asserts a file exists at the specified path
``` python
func setup():
	gut.file_touch('user://some_test_file')

func teardown():
	gut.file_delete('user://some_test_file')

func test_assert_file_exists():
	gut.p('-- passing --')
	assert_file_exists('res://addons/gut/gut.gd') # PASS
	assert_file_exists('user://some_test_file') # PASS

	gut.p('-- failing --')
	assert_file_exists('user://file_does_not.exist') # FAIL
	assert_file_exists('res://some_dir/another_dir/file_does_not.exist') # FAIL  
```
#### <a name="assert_file_does_not_exist"> assert_file_does_not_exist(file_path)
asserts a file does not exist at the specified path
``` python
func setup():
	gut.file_touch('user://some_test_file')

func teardown():
	gut.file_delete('user://some_test_file')

func test_assert_file_does_not_exist():
	gut.p('-- passing --')
	assert_file_does_not_exist('user://file_does_not.exist') # PASS
	assert_file_does_not_exist('res://some_dir/another_dir/file_does_not.exist') # PASS

	gut.p('-- failing --')
	assert_file_does_not_exist('res://addons/gut/gut.gd') # FAIL
```
#### <a name="assert_file_empty"> assert_file_empty(file_path)
asserts the specified file is empty
``` python
func setup():
	gut.file_touch('user://some_test_file')

func teardown():
	gut.file_delete('user://some_test_file')

func test_assert_file_empty():
	gut.p('-- passing --')
	assert_file_empty('user://some_test_file') # PASS

	gut.p('-- failing --')
	assert_file_empty('res://addons/gut/gut.gd') # FAIL
```
#### <a name="assert_file_not_empty"> assert_file_not_empty(file_path)
asserts the specified file is not empty
``` python
func setup():
	gut.file_touch('user://some_test_file')

func teardown():
	gut.file_delete('user://some_test_file')

func test_assert_file_not_empty():
	gut.p('-- passing --')
	assert_file_not_empty('res://addons/gut/gut.gd') # PASS

	gut.p('-- failing --')
	assert_file_not_empty('user://some_test_file') # FAIL
```
#### <a name="assert_extends"> assert_extends(object, a_class, text)
Asserts that "object" extends "a_class".  object must be an instance of an object.  It cannot be any of the built in classes like Array or Int or Float.  a_class must be a class, it can be loaded via load, a GDNative class such as Node or Label or anything else.

``` python
func test_assert_extends():
	gut.p('-- passing --')
	assert_extends(Node2D.new(), Node2D)
	assert_extends(Label.new(), CanvasItem)
	assert_extends(SubClass.new(), BaseClass)
	# Since this is a test script that inherits from test.gd, so
	# this passes.  It's not obvious w/o seeing the whole script
	# so I'm telling you.  You'll just have to trust me.
	assert_extends(self, load('res://addons/gut/test.gd'))

	var Gut = load('res://addons/gut/gut.gd')
	var a_gut = Gut.new()
	assert_extends(a_gut, Gut)

	gut.p('-- failing --')
	assert_extends(Node2D.new(), Node2D.new())
	assert_extends(BaseClass.new(), SubClass)
	assert_extends('a', 'b')
	assert_extends([], Node)
```
#### <a name="assert_get_set_methods"> assert_get_set_methods(obj, property, default, set_to)
I found that making tests for most getters and setters was repetitious and annoying.  Enter `assert_get_set_methods`.  This assertion handles 80% of your getter and setter testing needs.  Given an object and a property name it will verify:
 * The object has a method called `get_<PROPERTY_NAME>`
 * The object has a method called `set_<PROPERTY_NAME>`
 * The method `get_<PROPERTY_NAME>` returns the expected default value when first called.
 * Once you set the property, the `get_<PROPERTY_NAME>`will return the value passed in.

On the inside Gut actually performs up to 4 assertions.  So if everything goes right you will have four passing asserts each time you call `assert_get_set_methods`.  I say "up to 4 assertions" because there are 2 assertions to make sure the object has the methods and then 2 to verify they act correctly.  If the object does not have the methods, it does not bother running the tests for the methods.
``` python
class SomeClass:
	var _count = 0

	func get_count():
		return _count
	func set_count(number):
		_count = number

	func get_nothing():
		pass
	func set_nothing(val):
		pass

func test_assert_get_set_methods():
  var some_class = SomeClass.new()
  gut.p('-- passing --')
  assert_get_set_methods(some_class, 'count', 0, 20) # 4 PASSING

  gut.p('-- failing --')
  # 1 FAILING, 3 PASSING
  assert_get_set_methods(some_class, 'count', 'not_default', 20)  
  # 2 FAILING, 2 PASSING
  assert_get_set_methods(some_class, 'nothing', 'hello', 22)
  # 2 FAILING
  assert_get_set_methods(some_class, 'does_not_exist', 'does_not', 'matter')
```
#### gut.p(text, level=0, indent=0)
Print info to the GUI and console (if enabled).  You can see examples if this in the sample code above.  In order to be able to spot check the sample code, I print out a divider between the passing and failing tests.

#### gut.pause_before_teardown()
This method will cause Gut to pause before it moves on to the next test.  This is useful for debugging, for instance if you want to investigate the screen or anything else after a test has finished executing.  See also `set_ignore_pause_before_teardown`
#### yield_for(time_in_seconds)
This simplifies the code needed to pause the test execution for a number of seconds so the thing that you are testing can run its course in real time.  There are more details in the Yielding section.  It is designed to be used with the `yield` built in.  The following example will pause your test execution (and only the test execution) for 2 seconds before continuing.  You must call an assert or `pending` or `end_test()` after a yield or the test will never stop running.
``` python
class MovingNode:
	extends Node2D
	var _speed = 2

	func _ready():
		set_process(true)

	func _process(delta):
		set_pos(get_pos() + Vector2(_speed * delta, 0))

func test_illustrate_yield():
	var moving_node = MovingNode.new()
	add_child(moving_node)
	moving_node.set_pos(Vector2(0, 0))

	# While the yield happens, the node should move
	yield(yield_for(2), YIELD)
	assert_gt(moving_node.get_pos().x, 0)
	assert_between(moving_node.get_pos().x, 3.9, 4, 'it should move almost 4 whatevers at speed 2')
```
#### end_test()
This is a holdover from previous versions.  You should probably use an assert or `pending` to close out a yielded test but you can use this instead if you really really want to.
```
func test_illustrate_end_test():
	yield(yield_for(1), YIELD)
	# we don't have anything to test yet, or at all.  So we
	# call end_test so that Gut knows all the yielding has
	# finished.
	end_test()
```