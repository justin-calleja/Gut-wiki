The `double` method works similarly to `load`.  It will return a loaded class or scene that has empty implementations for all the methods defined in the script you pass it.  It will also include empty implementations for any methods in user-defined super classes.  It does not include implementations for any of the Godot Built-in super classes such as `Node2D` or `WindowDialog` (unless you have overloaded them in your script or in one of the user-defined super classes your script inherits from).  Actually, you can if you use the FULL Doubling strategy described at the bottom of this page.

All methods in a doubled object will return `null`.  You can change this behavior using [Stubbing](https://github.com/bitwes/Gut/wiki/Stubbing).  You can change methods to return specific values or even to call the original logic.

You can double Scripts, Inner Classes, and Packed Scenes.  Once you have a double, you can then call `new` or `instance` on it to create instances of a doubled object.  

Anything you `double` __must__ have an `_init` method that either takes 0 parameters or has defaults for all parameters.  Under the covers Gut will create an instance of the object you pass and then use Godot methods to get all the information about the object that Gut needs to create the double.

You can also create a [Partial Double](https://github.com/bitwes/Gut/wiki/PartialDouble) which will act exactly like its source object but you can spy and stub any method.  It's like the inverse of a double.

### Doubling a Script
To double a script just give it a path or an already loaded script.
``` python
const MY_SCRIPT_PATH = 'res://my_script.gd'
var MyScript = load(MY_SCRIPT_PATH)

# Load the doubled object.
var DoubledMyScript = double(MY_SCRIPT_PATH)
# or
var DoubledMyScript = double(MyScript)

# Create an instance of a doubled object
var doubled_script = DoubledMyScript.new()
# or
var doubled_script = double(MyScript).new()
```
### Doubling an Inner Class
When doubling an Inner Class you have to specify the path/script-object where the Inner Class is and then pass a `/` delimited list of Inner Classes that represents the hierarchy of the Inner Classes.

``` python
# -----------------------------------------------
# Given this as res://sripts/my_inners.gd
# -----------------------------------------------
class InnerA:
  var something = null

  class InnerA2:
    var something_else = null

class InnerB:
  var thing = null

# -----------------------------------------------
# You would double the various Inner Classes like this:
# -----------------------------------------------
const SCRIPT_WITH_INNERS_PATH = 'res://my_inners.gd'
var ScriptWithInners = load(SCRIPT_WITH_INNERS_PATH)

# Load the doubled objects
var DoubledInnerB = double(SCRIPT_WITH_INNERS_PATH, 'InnerB')
# or
var DoubledInnerA2 = double(ScriptWithInners, 'InnerA/InnerA2')

# Create an instance of a doubled inner class
var doubled_inner_a2 = DoubledInnerA2.new()
# or
var doubled_inner_b = double(SCRIPT_WITH_INNERS, 'InnerB').new()
```
When doubling Inners and passing a loaded class, you have to pass the class that contains the Inner Class(es).  Given the example above, you __CANNOT__ do `double(ScriptWithInners.InnerB)`.  You __MUST__ always pass the path to the inner classes.

### Doubling a Scene
A doubled version of your scene is created along with a double of its script.  The doubled scene is altered to load the doubled script instead of the original.  A reference to the newly doubled scene is returned.  You can call `instance` on the returned reference.

``` python
const MY_SCENE_PATH = 'res://my_scene.tscn'
var MyScene = load(MY_SCENE_PATH)

# Load up the doubled objects
var DoubledScene = double(MY_SCENE_PATH)
# or
var DoubledScene = double(MyScene)

# Create an instance
var doubled_scene = DoubledScene.instance()
# or
var doubled_scene = double(MY_SCENE_PATH).instance()
```

### Doubling Static Methods
Currently you cannot double static methods.  In fact if you try to double a class with a static method then you will get an error that looks similar to:
```
Parser Error: Function signature doesn't match the parent. Parent signature is: 'Variant foo()'.
```
As of now, GUT does not have the ability to detect static methods in the code.  As a workaround there is the `ignore_method_when_doubling` method.  This method takes in a variant as the first parameter and a method name as the second.  The first parameter can be a path to a script, a path to a scene, a loaded script, or a loaded scene.

Calling this method will prevent GUT from trying to make a stubbed out version of the method in the generated double allowing you to successfully double your classes that contain static methods.

These ignored methods are cleared after each test is ran to avoid any unexpected results in your tests, so you may want to add this call to your `before_each`.

There's more info and examples on this method on the [[Methods|Methods]] page.

### Doubling Built-Ins
You can `double` built-in objects that are not inherited by a script such as a `Node2D` or a `Raycast2D`.  These doubles are always created using the Doubling Strategy (see below) of "FULL" since there are not any overloaded methods.

For example you can `double` or `partial_double` like this:
``` python
var doubled_node2d = double(Node2D).new()
stub(doubled_node2d, 'get_position').to_return(-1)

var partial_doubled_raycast = partial_double(Raycast2D).new()
```

# What Do I Do With My Double?
Doubles are useful when you want to test an object that requires another object but you don't want to be deal with the overhead of other object's implementation.

Sometimes an empty implementation isn't enough.  Sometimes you need specific values or things to be returned from one of your doubled methods.  For this we have [Stubbing](https://github.com/bitwes/Gut/wiki/Stubbing).  Stubbing allows you to specify return values in various different scenarios such as "returning a 9 whenever a method is called" or "returning 57 when a method is called with the parameters 'a' and 24".

You can also [Spy](https://github.com/bitwes/Gut/wiki/Spies) on your double.  Once you have a double there are special `asserts` that can be used with it to verify that a method in the doubled object was called.  You can `assert` very specific calls too, such as with a specific list of parameters, or make sure it was called a certain number of times.

# The Fine Details

## Example of Script vs Built-in doubling
Only methods that have been defined in the script you pass in OR methods that have been defined in parent scripts get the empty implementation in the double.  Methods defined in any built-in Godot class do not get overridden __unless__ the script (or parent script(s)) have them implemented.

This is in the process of being changed but it is not fully implemented yet.  See the section on Double Strategies.

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

## Specifics About The Doubled Object
The doubled object that you get back will inherit from the object you specify.  This means that the object will have all the variables and Inner Classes defined in the object.  The variables will have the default values defined in the script.  Inner Classes in the source script will retain all of their functionality.  They are not doubled in any way.  This is because the Inner Classes that end up in your double are actually from the script it inherits from.  You can create doubles of specific Inner Classes but the Inner Classes in a doubled script are not altered in any way.

# Doubling Strategy
Remember all that stuff I said earlier about not being able to double Godot Built-Ins?  Forget about it...or forget half of it, maybe 45% of it.  

You can spy and stub most of the Built-Ins in Godot if you enable the `FULL` Doubling Strategy. I've enabled this feature in my own game and it didn't crash (I currently have 75 test scripts and 3633 asserts).  As reassuring as that was I'm still not sure that it won't blow up for someone so it is off by default.

The following methods cannot be spied on due to implementation details with either Gut or GDScript.  There might be more.  

```
has_method      _draw
get_script      _physics_process
get             _input
_notification   _unhandled_input
get_path        _unhandled_key_input
_enter_tree     _get
_exit_tree      emit_signal
_process        _set
```
##### Remember
If you've defined one of these methods in your class then you can double/spy on them just as you normally would.

## Setting the Doubling Strategy
You can set the default strategy from the command line, .gutconfig, or by calling `set_double_strategy` on your Gut instance.  

You can also override the default strategy at the Test Script level or for a specific call to `double`.  When set at the script level, it will reset at the end of the script or Inner Test Class.  When passed to `double` it will only take effect for that one double.

### .gutconfig
Valid values are `partial`(default) or `full`
```
"double_strategy":"full"
```
### Command Line
Use the `-gdouble_strategy` option with the values `partial` or `full`
```
-gdouble_strategy=full
```
### Script Level
```
set_double_strategy(DOUBLE_STRATEGY.FULL)
set_double_strategy(DOUBLE_STRATEGY.PARTIAL)
```
### When Calling `double`
Just add another parameter to your call to `double` using the `DOUBLE_STRATEGY` enum.  
```
double('res://thing.gd', DOUBLE_STRATEGY.PARTIAL)
double('res://inners.gd', 'InnerA', DOUBLE_STRATEGY.FULL)
double('res://my_scene.tscn', DOUBLE_STRATEGY.PARTIAL)
```

# Where to next?
* [Stubbing](https://github.com/bitwes/Gut/wiki/Stubbing)
* [Spies](https://github.com/bitwes/Gut/wiki/Spies)
