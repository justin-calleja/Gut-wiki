[__This feature is experimental__](https://github.com/bitwes/Gut/wiki/About-Experimental)

# What can you double
* Scripts via `double`.
  * All methods that do not come from built-in Godot classes will be stubbed to do nothing.  
  * All methods that come from built-in Godot classes will retain their original implementation even if overloaded in your class.
  * `_init` must have 0 parameters or the parameters must have default values.


* Packed Scenes via `double_scene`
  * All the methods in the scene's script that do not come from built-in Godot classes will be stubbed to do nothing.
  * All methods that come from built-in Godot classes will retain their original implementation even if overloaded in your class.
  * `_init` must have 0 parameters or the parameters must have default values.
  * The scene's script must be able to be instantiated with `new` without blowing up.

# What you cannot double
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

When stubbing doubled scenes, use the path to the scene, __not__ the path to the scene's script.  If you double and stub the script used by the scene, the `instance` you make from `double_scene` will not return values stubbed for the script.  It will only return values stubbed for the scene.

In order for a scene to be doubled, the scene's script must be able to be instantiated with `new` without erroring.

## Example
Given the script `res://the_script.gd`:
```
func return_hello():
  return 'hello'
```
And given a scene with the path `res://double_this_scene.tscn` which has its script set to `res://the_script.gd`.  

The following asserts will pass
``` python
func test_illustrate_stubbing_scenes():
  const SCENE_PATH = 'res://double_this_scene.tscn'
  const SCRIPT_PATH = 'res://the_script.gd'

  var scene = double_scene(SCENE_PATH).instance()
  var script = double(SCRIPT_PATH).new()

  stub(SCENE_PATH, 'return_hello').to_return('world')
  stub(SCRIPT_PATH, 'return_hello').to_return('goodbye')

  assert_eq(scene.return_hello(), 'world')
  assert_eq(scene.return_hello(), 'goodbye')
```
