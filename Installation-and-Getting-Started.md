## New Installs and Upgrades
Download and extract the zip from the [releases](https://github.com/bitwes/gut/releases) or from the [Godot Asset Library](https://godotengine.org/asset-library/asset/54).  

Extract the zip and place the `gut` directory into your `addons` directory in your project.  If you don't have an `addons` folder at the root of your project, then make one and THEN put the `gut` directory in there.

## New Install Configuration
From the menu choose Scene->Project Settings, click the plugins tab and activate Gut.

The next few steps cover the suggested configuration.  Feel free to deviate where you see fit.

1.  Create directories to store your tests and test related code
	* `res://test`
	* `res://test/unit`
	* `res://test/integration`
1.  Create a scene that will use Gut to run your tests at `res://test/tests.tscn`
	* Add a Gut object the same way you would any other object.
	* Click "Add/Create Node"
	* type "Gut"
	* press enter.
1.  Configure Gut to find your tests.  Select it in the Scene Tree and set the following settings in the Inspector:
	* In the `Directory1` setting enter `res://test/unit`
	* In the `Directory2` setting enter `res://test/integration`

That's it.  The next step is to make some tests.

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

