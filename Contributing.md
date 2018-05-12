This testing tool has tests of course.  All Gut related tests are found in the `test/unit` and `test/integration` directories.  Any enhancements or bug fixes should have a corresponding pull request with new tests.

The bulk of the tests for Gut are in [test_gut.gd](gut_tests_and_examples/test/unit/test_gut.gd) and [test_test.gd](gut_tests_and_examples/test/unit/test_test.gd).  [test_signal_watcher.gd](gut_tests_and_examples/test/unit/test_signal_watcher.gd) tests the class used to track the emitting of signals.  The other test scripts in `unit` and `integration` should be run and their output spot checked since they test other parts of Gut that aren't easily testabled.

For convenience, the `main.tscn` includes a handy "Run Gut Unit Tests" button that will kick off all the essential test scripts.
