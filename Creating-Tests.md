
# <a name="creating_tests"> Making Tests

# Sample for Setup
Here's a sample test script.  Copy the contents into the file `res://test/unit/test_example.gd` then run your scene.  If everything is setup correctly then you'll see some passing and failing tests.  If you don't have "Run on Load" checked in the editor, you'll have to hit the "Run" button on the dialog window.

``` python
extends "res://addons/gut/test.gd"
func before_each():
	gut.p("ran setup", 2)

func after_each():
	gut.p("ran teardown", 2)

func before_all():
	gut.p("ran run setup", 2)

func after_all():
	gut.p("ran run teardown", 2)

func test_assert_eq_number_not_equal():
	assert_eq(1, 2, "Should fail.  1 != 2")

func test_assert_eq_number_equal():
	assert_eq('asdf', 'asdf', "Should pass")

func test_assert_true_with_true():
	assert_true(true, "Should pass, true is true")

func test_assert_true_with_false():
	assert_true(false, "Should fail")

func test_something_else():
	assert_true(false, "didn't work")
```

# Creating tests
All test scripts must extend the test class.
* `extends "res://addons/gut/test.gd"`

Each test script has optional setup and teardown methods that you can provide abn implementation for.  These are called by Gut at various stages of execution.  They take no parameters.
 * `before_each()`:  Ran before each test (__formerly known as `setup`__)
 * `after_each()`:  Ran after each test (__formerly known as `teardown`__)
 * `before_all()`:  Ran before any test is run (__formerly known as `prerun_setup`__)
 * `after_all()`:  Ran after all tests have run (__formerly known as `postrun_teardown`__)

_* The old names for these methods still work but have been changed to make them more accessible.  Please see the 6.6.0 section of [CHANGES.md](https://github.com/bitwes/Gut/blob/master/CHANGES.md) for more information and some advice on refactoring existing tests._<br/>

All tests in the test script must start with the prefix `test_` in order for them to be run.  The methods must not have any parameters.
* `func test_this_is_only_a_test():`

Each test should perform at least one assert or call `pending` to indicate the test hasn't been implemented yet.

A list of all `asserts` and other helper functions available in your test script can be found in [Methods](https://github.com/bitwes/Gut/wiki/Methods).  There's also some helpful methods in the Gut object itself.  They are listed in [Gut Settings and Methods](https://github.com/bitwes/Gut/wiki/Gut-Settings-And-Methods)

# Inner Test Classes
You can group tests together using Inner Classes. These classes must start with the prefix `'Test'` (this is configurable) and must also extend `res://addons/gut/test.gd`.  You cannot create Inner Test Classes inside Inner Test Classes.  More info can be found at [Inner Test Classes](https://github.com/bitwes/Gut/wiki/Inner-Test-Classes).

## Simple Example
```
extends 'res://addons/gut/test.gd'

class TestFeatureA:
	extends 'res://addons/gut/test.gd'

	var Obj = load('res://scripts/object.gd')
	var _obj = null

	func before_each():
		_obj = Obj.new()

	func test_something():
		assert_true(_obj.is_something_cool(), 'Should be cool.')

class TestFeatureB:
	extends 'res://addons/gut/test.gd'

	var Obj = load('res://scripts/object.gd')
	var _obj = null

	func before_each():
		_obj = Obj.new()

	func test_foobar():
		assert_eq(_obj.foo(), 'bar', 'Foo should return bar')
```
# Where to next?
* [Gut Settings and Methods](https://github.com/bitwes/Gut/wiki/Gut-Settings-And-Methods)
* [Inner Test Classes](https://github.com/bitwes/Gut/wiki/Inner-Test-Classes)
* [Methods](https://github.com/bitwes/Gut/wiki/Methods)
* [Command Line](https://github.com/bitwes/Gut/wiki/Command-Line)
* [Simulate](https://github.com/bitwes/Gut/wiki/Simulate)
* [Yielding during tests](https://github.com/bitwes/Gut/wiki/Yielding)
