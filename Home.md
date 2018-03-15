# Gut 6.0.0
GUT (Godot Unit Test) is a utility for writing tests for your Godot Engine game.  It allows you to write tests for your gdscript in gdscript.

### Godot 3.0 Compatible.
Version 6.0.0 is Godot 3.0 compatible.  These changes are not compatible with any of the 2.x versions of Godot.  The godot_2x branch has been created to hold the old version of Gut that works with Godot 2.x.  Barring any severe issues, there will not be any more development for Godot 2.x.

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

#  <a name="contributing"> Contributing
This testing tool has tests of course.  All Gut related tests are found in the `test/unit` and `test/integration` directories.  Any enhancements or bug fixes should have a corresponding pull request with new tests.

The bulk of the tests for Gut are in [test_gut.gd](test/unit/test_gut.gd) and [test_test.gd](test/unit/test_test.gd).  [test_signal_watcher.gd](test/unit/test_signal_watcher.gd) tests the class used to track the emitting of signals.  The other test scripts in `unit` and `integration` should be run and their output spot checked since they test other parts of Gut that aren't easily testabled.

For convenience, the `main.tscn` includes a handy "Run Gut Unit Tests" button that will kick off all the essential test scripts.

# Who do I talk to?
You can talk to me, Butch Wesley

* Github:  bitwes
* Godot forums:  bitwes
