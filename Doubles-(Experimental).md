[__This feature is experimental__](https://github.com/bitwes/Gut/wiki/About-Experimental)

Doubles are useful when you want to test an object that requires another object but you don't want to be deal with the overhead of other object's implementation.  We want tests

The `double` function creates a new script that has empty overloads for all methods defined in the class.  You can use the object returned from `double` to create instances of the "doubled" script.  Once you have a "doubled" object you can stub return values via `stub` and make assertions about which methods have been called on the instance using `assert_called`, `assert_not_called`, and `assert_call_count`.

See the [stubbing](https://github.com/bitwes/Gut/wiki/Stubbing-(Experimental)) and [spy](https://github.com/bitwes/Gut/wiki/Spies-(Experimental)) pages for more information.

# There is one <u>VERY</u> important caveat.  
Only methods that have been defined in the script you pass in OR methods that have been defined in parent scripts get the empty implementation in the double.  Methods defined in any built-in Godot class do not get overridden __unless__ the script (or parent script(s)) have them implemented.

This will hopefully change in the future, but it's probably going to take a lot of trial and error...so no promises on when.

For example, given the following script at location `res://example.gd`
``` python
extends Node2D

var _value = 1
func set_position(pos):
  print('setting position')
  .set_position(pos)

func get_value():
  return _value

func set_value(value):
  _value = value
```
And, in a test, you double this class:
```
  var Doubled = double('res://example.gd')
  var doubled = Doubled.new()
```
Then:
* You can stub `get_value`, `set_value`, `set_position`.
* You __cannot__ stub `get_position` since this class does not implement it.  This will result in a runtime error.
* You can use `assert_method_called(doubled, 'get_value')`
* You __should not__ use `assert_method_called(doubled, 'get_position')`.  _You can but it will always return 0._


# What can you double
* Scripts via `double`.
  * All methods that do not come from built-in Godot classes will be stubbed to do nothing.  
  * All methods that come from built-in Godot classes will retain their original implementation even if overloaded in your class.
  * `_init` must have 0 parameters or the parameters must have default values.
* Packed Scenes via `double_scene` (.tscn files only)
  * All the methods in the scene's script that do not come from built-in Godot classes will be overridden to do nothing.
  * All methods that come from built-in Godot classes will retain their original implementation unless overloaded in your script.
  * `_init` must have 0 parameters or the parameters must have default values.
  * The scene's script must be able to be instantiated with `new` without blowing up.


# What you cannot double
* Scenes with the `.scn` extension.
* Inner Classes
* Any class whose `_init` method __requires__ parameters.  If all the parameters for `_init` have default values then it __will__ work.


# Doubling Scripts
Script doubles can be created using the `double` method.  This will return an object that can be instanced via `new`.  The doubled object will have all the methods defined in the source object but the implementation will be empty.  The doubled class inherits from the source object so it will have all the same variables and Inner Classes defined in the source object.  The Inner Classes will not be doubled, they will remain "as is".


## Example
As of now, only methods not defined in a built in class have their implementation changed.  For example given the script `res://scripts/double_this.gd`:

``` python
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
``` python
func test_doubling():
  var DoubledObj = double('res://scripts/double_this.gd')
  var doubled_inst = DoubledObj.new()
```
Then the methods `return_seven`, `return_hello` will be altered for `doubled_inst`.  The Inner Class `InnerClass` would exist but any methods would not be altered.  The member variables `foo` and `bar` will also exist in `doubled_inst` and they will have the same values as the source class.

Currently you __cannot__ double Inner Classes but should be implemented soon.

# Doubling Packed Scenes
Doubling packed scenes works very similar to doubling a script and is done using `double_scene`.  A doubled version of your scene is created along with a double of its script.  The doubled scene is altered to load the doubled script instead of the original.  A reference to the newly doubled scene is returned.  You can call `instance` on the returned reference.

# Where to next?
* [Stubbing](https://github.com/bitwes/Gut/wiki/Stubbing-(Experimental))
* [Spies](https://github.com/bitwes/Gut/wiki/Spies-(Experimental))
