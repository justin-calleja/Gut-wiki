There are pre-run and post-run hooks that you can use to run your own code before and/or after a test run.

`res://addons/gut/hook_script.gd`

`run` method

`gut` object

Do not put anything "time sensitive" in the `_init` method since this is called for both at the beginning of the run.  The `gut` variable will be null.  It is not populated until just before the `run` method is called.

All Hook scripts specified must be valid or the run will not be executed.
