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
* <b>Inner Class Prefix</b>:  This is the prefix that Gut will use to find Inner Classes that are test scripts.
* <b>Inner Class Name</b>:  Only run Inner Classes that have names that contain the specified string.  
* <b>Directory(1-6)</b>:  The path to the directories where your test scripts are located.  Subdirectories are not included.  If you need more than six directories you can use the `add_directory` method to add more.

## <a name="gut_methods"> Methods for Configuring the Execution of Tests
These methods would be used inside the scene you created at `res://test/tests.tcn`.  These methods can be called against the Gut node you created.  Most of these are not necessary anymore since you can configure Gut in the editor but they are here if you want to use them.  Simply put `get_node('Gut').` in front of any of them.  

<i>__**__ indicates the option can be set via the editor</i>
* `add_script(script, select_this_one=false)` add a script to be tetsted with test_scripts
* __**__`add_directory(path, prefix='test_', suffix='.gd')` add a directory of test scripts that start with prefix and end with suffix.  Subdirectories not included.  This method is useful if you have more than the 6 directories the editor allows you to configure.  You can use this to add others.
* __**__`test_scripts()` run all scripts added with add_script or add_directory.  If you leave this out of your script then you can select which script will run, but you must press the "run" button to start the tests.
* `test_script(script)` runs a single script immediately.
* __**__`select_script(script_name)` sets a script added with `add_script` or `add_directory` to be initially selected.  This allows you to run one script instead of all the scripts.  This will select the first script it finds that contains the specified string.
* `get_test_count()` return the number of tests run
* `get_assert_count()` return the number of assertions that were made
* `get_pass_count()` return the number of tests that passed
* `get_fail_count()` return the number of tests that failed
* `get_pending_count()` return the number of tests that were pending
* __**__`get/set_should_print_to_console(should)` accessors for printing to console
* `get_result_text()` returns all the text contained in the GUI
* `clear_text()` clears the text in the GUI
* `set_ignore_pause_before_teardown(should_ignore)` causes GUI to disregard any calls to pause_before_teardown.  This is useful when you want to run in a batch mode.
* __**__`set_yield_between_tests(should)` will pause briefly between every 5 tests so that you can see progress in the GUI.  If this is left out, it  can seem like the program has hung when running longer test sets.
* __**__`get/set_log_level(level)` see section on log level for list of values.
* __**__`disable_strict_datatype_checks(true)` disables strict datatype checks.  See section on "Strict type checking" before disabling.

# <a name="extras"> Extras

##  <a name="strict"> Strict type checking
Gut performs type checks in the asserts when comparing two different types that would normally cause a runtime error.  With the type checking enabled (on be default) your test will fail instead of crashing.  Some types are ok to be compared such as Floats and Integers but if you attempt to compare a String with a Float your test will fail instead of blowing up.

You can disable this behavior if you like by calling `disable_strict_datatype_checks(true)` on your Gut node or by clicking the checkbox to "Disable Strict Datatype Checks" in the editor.

##  <a name="files"> File Manipulation Methods for Tests
Use these methods in a test or setup/teardown method to make file related testing easier.  These all exist on the Gut object so they must be prefixed with `gut`
* `gut.file_touch(path)` create an empty file if it doesn't exist.
* `gut.file_delete(path)` delete a file
* `gut.is_file_empty(path)` checks if a file is empty
* `gut.directory_delete_files` deletes all files in a directory.  does not delete subdirectories or any files in them.

##  <a name="watch"> Watching tests as they execute
When running longer tests it can appear as though the program or Gut has hung.  To address this and see the tests as they execute, a short yield was added between tests.  To enable this feature call `set_yield_between_tests(true)` before running your tests or use the "Yield Between Tests" in the Editor.

##  <a name="output_detail"> Output Detail
The level of detail that is printed to the screen can be changed using the slider on the dialog or by calling `set_log_level` with one of the following constants defined in Gut

* LOG_LEVEL_FAIL_ONLY (0)
* LOG_LEVEL_TEST_AND_FAILURES (1)
* LOG_LEVEL_ALL_ASSERTS (2)

##  <a name="printing"> Printing info
The `gut.p` method allows you to print information out indented under the test output.  It has an optional 2nd parameter that sets which log level to display it at.  Use one of the constants in the section above to set it.  The default is `LOG_LEVEL_FAIL_ONLY` which means the output will always be visible.  
