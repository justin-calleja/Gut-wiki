
# <a name="creating_tests"> Making Tests

## Sample for Setup
Here's a sample test script.  Copy the contents into the file `res://test/unit/test_example.gd` then run your scene.  If everything is setup correctly then you'll see some passing and failing tests.  If you don't have "Run on Load" checked in the editor, you'll have to hit the "Run" button on the dialog window.

``` python
extends "res://addons/gut/test.gd"
func setup():
	gut.p("ran setup", 2)

func teardown():
	gut.p("ran teardown", 2)

func prerun_setup():
	gut.p("ran run setup", 2)

func postrun_teardown():
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

## Creating tests
All test scripts must extend the test class.
* `extends "res://addons/gut/test.gd"`

Each test script has optional setup and teardown methods that are called at various stages of execution.  They take no parameters.
 * `setup()`:  Ran before each test
 * `teardown()`:  Ran after each test
 * `prerun_setup()`:  Ran before any test is run
 * `postrun_teardown()`:  Ran after all tests have run

All tests in the test script must start with the prefix `test_` in order for them to be run.  The methods must not have any parameters.
* `func test_this_is_only_a_test():`

Each test should perform at least one assert or call `pending` to indicate the test hasn't been implemented yet.

# Where to next?
* [Gut Settings and Methods](https://github.com/bitwes/Gut/wiki/Gut-Settings-And-Methods.md)
* [Inner Test Classes](https://github.com/bitwes/Gut/wiki/Inner-Test-Classes.md)
* [Methods](https://github.com/bitwes/Gut/wiki/Methods.md)
* [Command Line](https://github.com/bitwes/Gut/wiki/Command-Line.md)
* [Simulate](https://github.com/bitwes/Gut/wiki/Simulate)
* [Yielding during tests](https://github.com/bitwes/Gut/wiki/Yielding)
