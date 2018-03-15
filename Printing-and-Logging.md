##  <a name="watch"> Watching tests as they execute
When running longer tests it can appear as though the program or Gut has hung.  To address this and see the tests as they execute, a short yield was added between tests.  To enable this feature call `set_yield_between_tests(true)` before running your tests or use the "Yield Between Tests" in the Editor.

##  <a name="output_detail"> Output Detail
The level of detail that is printed to the screen can be changed using the slider on the dialog or by calling `set_log_level` with one of the following constants defined in Gut

* LOG_LEVEL_FAIL_ONLY (0)
* LOG_LEVEL_TEST_AND_FAILURES (1)
* LOG_LEVEL_ALL_ASSERTS (2)

##  <a name="printing"> Printing info
The `gut.p` method allows you to print information out indented under the test output.  It has an optional 2nd parameter that sets which log level to display it at.  Use one of the constants in the section above to set it.  The default is `LOG_LEVEL_FAIL_ONLY` which means the output will always be visible.  

