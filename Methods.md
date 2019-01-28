These are all the methods, bells, whistles and blinky lights you get when you extend the Gut Test Class (`extends "res://addons/gut/test.gd"`).  

All sample code listed for the methods can be found here in [test_readme_examples.gd](gut_tests_and_examples/test/samples/test_readme_examples.gd)

# Utilities
<table><tr>
<td>
TODO sort these lines
[end_test](#end_test)<br/>
[get_signal_emit_count](#get_signal_emit_count)<br/>
[get_signal_parameters](#get_signal_parameters)<br/>
[pending](#pending)<br/>
[watch_signals](#watch_signals)<br/>
[yield_for](#yield_for)<br/>
[yield_to](#yield_to)<br/>

[gut.p](#gut_p)<br/>
[pause_before_teardown](#pause_before_teardown)<br/>

[double](#double)<br/> TODO
[stub](#stub)<br/> TODO
[simulate](#simulate) TODO
</td>
</tr>

# Assertions
<table><tr>
<td>
TODO re-balance table
[assert_accessors](#assert_accessors)<br/>
[assert_almost_eq](#assert_almost_eq)<br/>
[assert_almost_ne](#assert_almost_ne)<br/>
[assert_between](#assert_between)<br/>
[assert_call_count](#assert_call_count)<br/>
[assert_called](#assert_called)<br/>
[assert_does_not_have](#assert_does_not_have)<br/>
[assert_eq (equal)](#assert_eq)<br/>
[assert_exports](#assert_exports)<br/>
[assert_false](#assert_false)<br/>
[assert_file_does_not_exist](#assert_file_does_not_exist)<br/>
[assert_file_empty](#assert_file_empty)<br/>
[assert_file_exists](#assert_file_exists)<br/>
[assert_file_not_empty](#assert_file_not_empty)<br/>
[assert_gt (greater than)](#assert_gt)<br/>
[assert_has_method](#assert_has_method)<br/>
[assert_has_signal](#assert_has_signal)<br/>
[assert_has](#assert_has)<br/>
[assert_is](#assert_is)<br/>

</td><td>

[assert_lt (less than)](#assert_lt)<br/>
[assert_ne (not equal)](#assert_ne)<br/>
[assert_not_called](#assert_not_called)<br/>
[assert_signal_emit_count](#assert_signal_emit_count)<br/>
[assert_signal_emitted_with_parameters](#assert_signal_emitted_with_parameters)<br/>
[assert_signal_emitted](#assert_signal_emitted)<br/>
[assert_signal_not_emitted](#assert_signal_not_emitted)<br/>
[assert_string_contains](#assert_string_contains)<br/>
[assert_string_ends_with](#assert_string_ends_with)<br/>
[assert_string_starts_with](#assert_string_starts_with)<br/>
[assert_true](#assert_true)<br/>


</td>
</tr></table>

#### <a name="pending"> pending(text="")
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
#### <a name="assert_almost_eq"> assert_almost_eq(got, expected, error_interval, text='')
Asserts that `got` is within the range of `expected` +/- `error_interval`.  The upper and lower bounds are included in the check.  Verified to work with integers, floats, and Vector2.  Should work with anything that can be added/subtracted.

``` python
gut.p('-- passing --')
assert_almost_eq(0, 1, 1, '0 within range of 1 +/- 1') # PASS
assert_almost_eq(2, 1, 1, '2 within range of 1 +/- 1') # PASS

assert_almost_eq(1.2, 1.0, .5, '1.2 within range of 1 +/- .5') # PASS
assert_almost_eq(.5, 1.0, .5, '.5 within range of 1 +/- .5') # PASS

assert_almost_eq(Vector2(.5, 1.5), Vector2(1.0, 1.0), Vector2(.5, .5))  # PASS

gut.p('-- failing --')
assert_almost_eq(1, 3, 1, '1 outside range of 3 +/- 1') # FAIL
assert_almost_eq(2.6, 3.0, .2, '2.6 outside range of 3 +/- .2') # FAIL

assert_almost_eq(Vector2(.5, 1.5), Vector2(1.0, 1.0), Vector2(.25, .25))  # PASS
```
#### <a name="assert_almost_ne"> assert_almost_ne(got, expected, error_interval, text='')
This is the inverse of `assert_almost_eq`.  This will pass if `got` is outside the range of `expected` +/- `error_interval`.
``` python
gut.p('-- passing --')
assert_almost_ne(1, 3, 1, '1 outside range of 3 +/- 1') # PASS
assert_almost_ne(2.6, 3.0, .2, '2.6 outside range of 3 +/- .2') # PASS

assert_almost_ne(Vector2(.5, 1.5), Vector2(1.0, 1.0), Vector2(.25, .25))  # PASS

gut.p('-- failing --')
assert_almost_ne(0, 1, 1, '0 within range of 1 +/- 1') # FAIL
assert_almost_ne(2, 1, 1, '2 within range of 1 +/- 1') # FAIL

assert_almost_ne(1.2, 1.0, .5, '1.2 within range of 1 +/- .5') # FAIL
assert_almost_ne(.5, 1.0, .5, '.5 within range of 1 +/- .5') # FAIL

assert_almost_ne(Vector2(.5, 1.5), Vector2(1.0, 1.0), Vector2(.5, .5))  # FAIL
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

#### <a name="assert_string_contains"> assert_string_contains(text, search, match_case=true)
Assert that `text` contains `search`.  Can perform case insensitive search by passing false for `match_case`.
```python
func test_string_contains():
	gut.p('-- passing --')
	assert_string_contains('abc 123', 'a')
	assert_string_contains('abc 123', 'BC', false)
	assert_string_contains('abc 123', '3')

	gut.p('-- failing --')
	assert_string_contains('abc 123', 'A')
	assert_string_contains('abc 123', 'BC')
	assert_string_contains('abc 123', '012')
```

#### <a name="assert_string_starts_with"> assert_string_starts_with(text, search, match_case=true)
Assert that `text` starts with `search`.  Can perform case insensitive check by passing false for `match_case`
```python
func test_string_starts_with():
	gut.p('-- passing --')
	assert_string_starts_with('abc 123', 'a')
	assert_string_starts_with('abc 123', 'ABC', false)
	assert_string_starts_with('abc 123', 'abc 123')

	gut.p('-- failing --')
	assert_string_starts_with('abc 123', 'z')
	assert_string_starts_with('abc 123', 'ABC')
	assert_string_starts_with('abc 123', 'abc 1234')
```

#### <a name="assert_string_ends_with"> assert_string_ends_with(text, search, match_case=true)
Assert that `text` ends with `search`.  Can perform case insensitive check by passing false for `match_case`
```python
func test_string_ends_with():
	gut.p('-- passing --')
	assert_string_ends_with('abc 123', '123')
	assert_string_ends_with('abc 123', 'C 123', false)
	assert_string_ends_with('abc 123', 'abc 123')

	gut.p('-- failing --')
	assert_string_ends_with('abc 123', '1234')
	assert_string_ends_with('abc 123', 'C 123')
	assert_string_ends_with('abc 123', 'nope')
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
func before_each():
	gut.file_touch('user://some_test_file')

func after_each():
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
func before_each():
	gut.file_touch('user://some_test_file')

func after_each():
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
func before_each():
	gut.file_touch('user://some_test_file')

func after_each():
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
func before_each():
	gut.file_touch('user://some_test_file')

func after_each():
	gut.file_delete('user://some_test_file')

func test_assert_file_not_empty():
	gut.p('-- passing --')
	assert_file_not_empty('res://addons/gut/gut.gd') # PASS

	gut.p('-- failing --')
	assert_file_not_empty('user://some_test_file') # FAIL
```
#### <a name="assert_is"> assert_is(object, a_class, text)
__Formerly known as `assert_extends`__<br/>
Asserts that "object" extends "a_class".  object must be an instance of an object.  It cannot be any of the built in classes like Array or Int or Float.  a_class must be a class, it can be loaded via load, a GDNative class such as Node or Label or anything else.

``` python
func test_assert_is():
	gut.p('-- passing --')
	assert_is(Node2D.new(), Node2D)
	assert_is(Label.new(), CanvasItem)
	assert_is(SubClass.new(), BaseClass)
	# Since this is a test script that inherits from test.gd, so
	# this passes.  It's not obvious w/o seeing the whole script
	# so I'm telling you.  You'll just have to trust me.
	assert_is(self, load('res://addons/gut/test.gd'))

	var Gut = load('res://addons/gut/gut.gd')
	var a_gut = Gut.new()
	assert_is(a_gut, Gut)

	gut.p('-- failing --')
	assert_is(Node2D.new(), Node2D.new())
	assert_is(BaseClass.new(), SubClass)
	assert_is('a', 'b')
	assert_is([], Node)
```

#### <a name="assert_exports">assert_exports(obj, property_name, type)
Asserts that `obj` exports a property with the name `property_name` and a type of `type`.  The `type` must be one of the various Godot built-in `TYPE_` constants.

``` python
class ExportClass:
	export var some_number = 5
	export(PackedScene) var some_scene
	var some_variable = 1

func test_assert_exports():
	var obj = ExportClass.new()

	gut.p('-- passing --')
	assert_exports(obj, "some_number", TYPE_INT)
	assert_exports(obj, "some_scene", TYPE_OBJECT)

	gut.p('-- failing --')
	assert_exports(obj, 'some_number', TYPE_VECTOR2)
	assert_exports(obj, 'some_scene', TYPE_AABB)
	assert_exports(obj, 'some_variable', TYPE_INT)
```

#### <a name="assert_called">assert_called(inst, method_name, parameters=null)
This assertion is is one of the ways Gut implements Spies.  It requires that you pass it an instance of a "doubled" object.  An instance created with `double` will record when a method it has is called.  You can then make assertions based on this.  

This assert will check the object to see if a call to the specified method (optionally with parameters) was called over the course of the test.  If it finds a match this test will `pass`, if not it will `fail`.

The `parameters` parameter is an array of values that you expect to have been passed to `method_name`.  If you do not specify any parameters then any call to `method_name` will match and the assert will `pass`.  If you specify parameters then all the parameter values must match.  You must specify all parameters the method takes, even if they have defaults.  Gut is not able (yet?) to fill in default values.

__Methods that are inherited from built-in parent classes are not yet recorded.  For example, you cannot make "called" assertions on methods like `set_position` unless your sub-class specifically implements it.  But since those methods retain their built-in functionality, you can just make normal assertions on them.__

``` python
# Given the following class located at 'res://test/doubler_test_objects/double_extends_node2d.gd'
# --------------------
	extends Node2D
	var _value = 0

	func get_value():
	    return _value
	func set_value(val):
	    _value = val
	func has_one_param(one):
	    pass
	func has_two_params_one_default(one, two=null):
	    pass
	func get_position():
	    return .get_position()
# --------------------

# This is how assert_called behaves
func test_assert_called():
	var DOUBLE_ME_PATH = 'res://test/doubler_test_objects/double_extends_node2d.gd'

	var doubled = double(DOUBLE_ME_PATH).new()
	doubled.set_value(4)
	doubled.set_value(5)
	doubled.has_two_params_one_default('a')
	doubled.has_two_params_one_default('a', 'b')

	gut.p('-- passing --')
	assert_called(doubled, 'set_value')
	assert_called(doubled, 'set_value', [5])
	# note the passing of `null` here.  Default parameters must be supplied.
	assert_called(doubled, 'has_two_params_one_default', ['a', null])
	assert_called(doubled, 'has_two_params_one_default', ['a', 'b'])

	gut.p('-- failing --')
	assert_called(doubled, 'get_value')
	assert_called(doubled, 'set_value', ['nope'])
	# This fails b/c Gut isn't smart enough to fill in default values for you...
	# ast least not yet.
	assert_called(doubled, 'has_two_params_one_default', ['a'])
	# This fails with a specific message indicating that you have to pass an
	# instance of a doubled class.
	assert_called(GDScript.new(), 'some_method')
```
#### <a name="asssert_not_called">assert_not_called(inst, method_name, parameters=null)
This is the inverse of `assert_called` and works the same way except, you know, inversely.  Matches are found based on parameters in the same fashion.  If a matching call is found then this assert will `fail`, if not it will `pass`.

#### <a name="assert_call_count">assert_call_count(inst, method_name, expected_count, parameters=null)
This assertion is is one of the ways Gut implements Spies.  It requires that you pass it an instance of a "doubled" object.  An instance created with `double` will record when a method it has is called.  You can then make assertions based on this.  

This asserts that a method on a doubled instance has been called a number of times.  If you do not specify any parameters then all calls to the method will be counted.  If you specify parameters, then only those calls that were passed matching values will be counted.

The `parameters` parameter is an array of values that you expect to have been passed to `method_name`.  If you do not specify any parameters then any call to `method_name` will match and the assert will `pass`.  If you specify parameters then all the parameter values must match.  You must specify all parameters the method takes, even if they have defaults.  Gut is not able (yet?) to fill in default values.

__Methods that are inherited from built-in parent classes are not yet recorded.  For example, you cannot make "call" assertions on methods like `set_position` unless your sub-class specifically implements it (you can, they will just always return 0).  Since those methods retain their built-in functionality, you can just make normal assertions on them.__


``` python
# Given the following class located at 'res://test/doubler_test_objects/double_extends_node2d.gd'
# --------------------
	extends Node2D
	var _value = 0

	func get_value():
	    return _value
	func set_value(val):
	    _value = val
	func has_one_param(one):
	    pass
	func has_two_params_one_default(one, two=null):
	    pass
	func get_position():
	    return .get_position()
# --------------------

# This is how assert_call_count behaves
func test_assert_call_count():
	var DOUBLE_ME_PATH = 'res://test/doubler_test_objects/double_extends_node2d.gd'

	var doubled = double(DOUBLE_ME_PATH).new()
	doubled.set_value(4)
	doubled.set_value(5)
	doubled.has_two_params_one_default('a')
	doubled.has_two_params_one_default('a', 'b')
	doubled.set_position(Vector2(100, 100))

	gut.p('-- passing --')
	assert_call_count(doubled, 'set_value', 2)
	assert_call_count(doubled, 'set_value', 1, [4])
	# note the passing of `null` here.  Default parameters must be supplied.
	assert_call_count(doubled, 'has_two_params_one_default', 1, ['a', null])
	assert_call_count(doubled, 'get_value', 0)

	gut.p('-- failing --')
	assert_call_count(doubled, 'set_value', 5)
	assert_call_count(doubled, 'set_value', 2, [4])
	assert_call_count(doubled, 'get_value', 1)
	# This fails with a specific message indicating that you have to pass an
	# instance of a doubled class even though technically the method was called.
	assert_call_count(GDScript.new(), 'some_method', 0)
	# This fails b/c double_extends_node2d does not have it's own implementation
	# of set_position.  The function is supplied by the parent class and these
	# methods are not yet being recorded.
	assert_call_count(doubled, 'set_position', 1)

```

#### <a name="assert_has_method">assert_has_method(obj, method)
Asserts that the passed in object has a method named `method`.
```python
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

func test_assert_has_method():
	var some_class = SomeClass.new()
	gut.p('-- passing --')
	assert_has_method(some_class, 'get_nothing')
	assert_has_method(some_class, 'set_count')

	gut.p('-- failing --')
	assert_has_method(some_class, 'method_does_not_exist')
```
#### <a name="assert_accessors"> assert_accessors(obj, property, default, set_to)
__Formerly known as `assert_get_set_methods`.__<br/>
I found that making tests for most getters and setters was repetitious and annoying.  Enter `assert_accessors`.  This assertion handles 80% of your getter and setter testing needs.  Given an object and a property name it will verify:
 * The object has a method called `get_<PROPERTY_NAME>`
 * The object has a method called `set_<PROPERTY_NAME>`
 * The method `get_<PROPERTY_NAME>` returns the expected default value when first called.
 * Once you set the property, the `get_<PROPERTY_NAME>`will return the value passed in.

On the inside Gut actually performs up to 4 assertions.  So if everything goes right you will have four passing asserts each time you call `assert_accessors`.  I say "up to 4 assertions" because there are 2 assertions to make sure the object has the methods and then 2 to verify they act correctly.  If the object does not have the methods, it does not bother running the tests for the methods.
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

func test_assert_accessors():
  var some_class = SomeClass.new()
  gut.p('-- passing --')
  assert_accessors(some_class, 'count', 0, 20) # 4 PASSING

  gut.p('-- failing --')
  # 1 FAILING, 3 PASSING
  assert_accessors(some_class, 'count', 'not_default', 20)  
  # 2 FAILING, 2 PASSING
  assert_accessors(some_class, 'nothing', 'hello', 22)
  # 2 FAILING
  assert_accessors(some_class, 'does_not_exist', 'does_not', 'matter')
```
#### <a name="gut_p"> gut.p(text, level=0, indent=0)
Print info to the GUI and console (if enabled).  You can see examples if this in the sample code above.  In order to be able to spot check the sample code, I print out a divider between the passing and failing tests.

#### <a name="pause_before_teardown"> pause_before_teardown()
This method will cause Gut to pause before it moves on to the next test.  This is useful for debugging, for instance if you want to investigate the screen or anything else after a test has finished executing.  See also `set_ignore_pause_before_teardown`

#### <a name="yield_for"> yield_for(time_in_seconds)
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

#### <a name="yield_to"> yield_to(object, signal_name, max_time)
`yield_to` allows you to yield to a signal just like `yield` but for a maximum amount of time.  This keeps tests moving along when signals are not emitted.  Just like with any test that has a yield in it, you must call an assert or `pending` or `end_test()` after a yield or the test will never stop running.

As a bonus, `yield_to` does an implicit call to `watch_signals` so you can easily make signal based assertions afterwards.
``` python
class TimedSignaler:
	extends Node2D
	var _time = 0

	signal the_signal
	func _init(time):
		_time = time

	func start():
		var t = Timer.new()
		add_child(t)
		t.set_wait_time(_time)
		t.connect('timeout', self, '_on_timer_timeout')
		t.set_one_shot(true)
		t.start()

	func _on_timer_timeout():
		emit_signal('the_signal')

func test_illustrate_yield_to_with_less_time():
	var t = TimedSignaler.new(5)
	add_child(t)
	t.start()
	yield(yield_to(t, 'the_signal', 1), YIELD)
	# since we setup t to emit after 5 seconds, this will fail because we
	# only yielded for 1 second via yield_to
	assert_signal_emitted(t, 'the_signal', 'This will fail')

func test_illustrate_yield_to_with_more_time():
	var t = TimedSignaler.new(1)
	add_child(t)
	t.start()
	yield(yield_to(t, 'the_signal', 5), YIELD)
	# since we wait longer than it will take to emit the signal, this assert
	# will pass
	assert_signal_emitted(t, 'the_signal', 'This will pass')
```
#### <a name="end_test"> end_test()
This is a holdover from previous versions.  You should probably use an assert or `pending` to close out a yielded test but you can use this instead if you really really want to.
``` python
func test_illustrate_end_test():
	yield(yield_for(1), YIELD)
	# we don't have anything to test yet, or at all.  So we
	# call end_test so that Gut knows all the yielding has
	# finished.
	end_test()
```
