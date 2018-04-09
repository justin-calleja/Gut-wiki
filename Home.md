# Gut 6.2.0
GUT (Godot Unit Test) is a utility for writing tests for your Godot Engine game.  It allows you to write tests for your gdscript in gdscript.

### Godot 3.0 Compatible.
Version 6.0.0 is Godot 3.0 compatible.  These changes are not compatible with any of the 2.x versions of Godot.  The godot_2x branch has been created to hold the old version of Gut that works with Godot 2.x.  Barring any severe issues, there will not be any more development for Godot 2.x.

# Getting Started
Here's a couple links to get you started.
* [Install](https://github.com/bitwes/Gut/wiki/Install.md)
* [Setup](https://github.com/bitwes/Gut/wiki/Setup.md)
* [Methods](https://github.com/bitwes/Gut/wiki/Methods.md)

# License
Gut is provided under the MIT license.  [The license is distributed with Gut so it is in the `addons/gut` folder](https://github.com/bitwes/Gut/addons/gut/LICENSE.md).  

# Contributing
This testing tool has tests of course.  All Gut related tests are found in the `test/unit` and `test/integration` directories.  Any enhancements or bug fixes should have a corresponding pull request with new tests.

The bulk of the tests for Gut are in [test_gut.gd](gut_tests_and_examples/test/unit/test_gut.gd) and [test_test.gd](gut_tests_and_examples/test/unit/test_test.gd).  [test_signal_watcher.gd](gut_tests_and_examples/test/unit/test_signal_watcher.gd) tests the class used to track the emitting of signals.  The other test scripts in `unit` and `integration` should be run and their output spot checked since they test other parts of Gut that aren't easily testabled.

For convenience, the `main.tscn` includes a handy "Run Gut Unit Tests" button that will kick off all the essential test scripts.

# Who do I talk to?
You can talk to me, Butch Wesley

* Github:  bitwes
* Godot forums:  bitwes
