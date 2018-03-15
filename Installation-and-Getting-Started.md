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

# <a name="gut_settings"> Gut Settings
The following settings are accessible in the Editor under "Script Variables"

* <b>Run On Load</b>:  Flag to indicate if Gut should start running tests when loaded.
* <b>Select Script</b>:  Select the named script in the drop down.  When this is set and "Run On Load" is true, only this script will be run.
* <b>Tests Like</b>:  Only tests that contain the set text will be run initially.
* <b>Should Print To Console</b>:  Print output to the console as well as to Gut.
* <b>Log Level</b>:  Set the level of output.
* <b>Yield Between Tests</b>:  A short yield is performed by Gut so that the Gut control has a chance to redraw.  This increases execution time by a tiny bit, but stops Gut from appearing to be hung up while it runs tests.
* <b>Disable Strict Datatype Checks</b>:  Disables the verifying of datatypes before comparisons are done.  You can disable this if you want.  See the section on datatype checks for more details.
* <b>Test Prefix</b>:  The prefix used on all test functions.  This prefixed will be used by Gut to find tests inside your test scripts.
* <b>File Prefix</b>:  The prefix used on all test files.  This is used in conjunction with the Directory settings to find tests.
* <b>File Extension</b>:  This is the suffix it will use to find test files.  
* <b>Directory(1-6)</b>:  The path to the directories where your test scripts are located.  Subdirectories are not included.  If you need more than six directories you can use the `add_directory` method to add more.

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

