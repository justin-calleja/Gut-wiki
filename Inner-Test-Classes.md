You can define test classes inside a test script that will be treated as test scripts themselves.  This allows you to create different contexts for your tests in a single script.  These Inner Classes have thier own `prerun_setup`, `setup`, `teardown`, and `postrun_teardown` methods that will be called.  Only the methods defined in the class are used, the methods defined in the containing script will not be called.

The Inner Classes must also extend `res://addons/gut/test.gd` and their constructor cannot take any parameters.  The Classes will be loaded and ran in the order they are defined _after_ all the tests in the containing script are run.

The order the tests are run are not guaranteed to be in the same order they are defined.  Also the line number for the tests will cannot currently be reported when they fail or are pending.

# Example

```
func prerun_setup():
	gut.p('script:  pre-run')

func setup():
	gut.p('script:  setup')

func teardown():
	gut.p('script:  teardown')

func postrun_teardown():
	gut.p('script:  post-run')

func test_something():
	assert_true(true)

class TestClass1:
	extends "res://addons/gut/test.gd"

	func prerun_setup():
		gut.p('TestClass1:  pre-run')

	func setup():
		gut.p('TestClass1:  setup')

	func teardown():
		gut.p('TestClass1:  teardown')

	func postrun_teardown():
		gut.p('TestClass1:  post-run')

	func test_context1_one():
		assert_true(true)

	func test_context1_two():
		pending()

  ```
This will generate the follwoing output
```
script:  pre-run
* test_something
    script:  setup
    PASSED:
    script:  teardown
/----
Testing Inner Class TestClass1
----/
TestClass1:  pre-run
* test_context1_two
    TestClass1:  setup
    Pending
    TestClass1:  teardown
* test_context1_one
    TestClass1:  setup
    PASSED:
    TestClass1:  teardown
    TestClass1:  post-run
```
