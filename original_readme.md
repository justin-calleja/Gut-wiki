



# Table of Contents
1.  [Install](#install)
1.  [Gut Settings](#gut_settings)
1.  [Creating Tests](#creating_tests)
1.  [Test Related Methods](#method_list)
1.  [Methods for Configuring Test Execution](#gut_methods)
1.  [Extras](#extras)
	1.  [Strict Type Checking](#strict)
	1.  [File Manipulation](#files)
	1.  [Watching Tests](#watch)
	1.  [Output Detail](#output_detail)
	1.  [Printing](#printing)
1. [Advanced](#advanced)
	1.  [Simulate](#simulate)
	1.  [Yielding](#yielding)
1. [Command Line Interface](#command_line)
1. [Contributing](#contributing)



#  <a name="advanced"> Advanced Testing




#  <a name="command_line"> Running Gut from the Command Line



#  <a name="contributing"> Contributing
This testing tool has tests of course.  All Gut related tests are found in the `test/unit` and `test/integration` directories.  Any enhancements or bug fixes should have a corresponding pull request with new tests.

The bulk of the tests for Gut are in [test_gut.gd](gut_tests_and_examples/test/unit/test_gut.gd) and [test_test.gd](gut_tests_and_examples/test/unit/test_test.gd).  [test_signal_watcher.gd](gut_tests_and_examples/test/unit/test_signal_watcher.gd) tests the class used to track the emitting of signals.  The other test scripts in `unit` and `integration` should be run and their output spot checked since they test other parts of Gut that aren't easily testabled.

For convenience, the `main.tscn` includes a handy "Run Gut Unit Tests" button that will kick off all the essential test scripts.

# Who do I talk to?
You can talk to me, Butch Wesley

* Github:  bitwes
* Godot forums:  bitwes
