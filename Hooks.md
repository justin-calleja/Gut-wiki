# Overview
GUT has pre-run and post-run hooks that allow you to take any initialization steps or verify the results of the run.

Hook scripts can be set on a GUT instance in a scene, through a command line option or in the `.gutconfig.json` file.

All Hook scripts must inherit from [`res://addons/gut/hook_scirpt.gd`](https://github.com/bitwes/Gut/blob/master/addons/gut/hook_script.gd).  If the pre-run or post-run scripts specified do not exist or do not extend `res://addons/gut/hook_scirpt.gd` then the run will be aborted before any tests are run.

All Hook scripts have access to the GUT instance that is running the tests via the `gut` property defined in `hook_script.gd`.  This is set after initializing the script.

GUT executes the `run()` method in  each of the Hook scripts.  Place your  custom code in there.  *Note* all Hook scripts are instantiated at the start of the run and later `run()` is called on each instance at the appropriate time.  The `_init()` method of your Hook Script will not have access to the GUT instance since it is set  later.

# Setup
Create scripts that inherit from `res://addons/gut/hook_script.gd`, implement the `run()` method, and set their paths.

#### Editor
The GUT node has `pre_run_script` and `post_run_script` properties to set the path to your Hook scripts.

#### Command Line
The command line `-gpre_run_script` and `-gpost_run_script` options.  You can also specify `pre_run_script` and `post_run_script` in the `.gutconfig.json` file.

# Pre-Run Hook
The pre-run hook is run just before any tests are executed.  This can be useful in setting global variables or performing any setup required for all your tests.

The post-run hook can access the pre-run hook instance via `gut.get_pre_run_script_instance()`.

Possible uses:
* mute all sounds `AudioServer.set_bus_volume_db(0, -INF)`
* set flags you've implemented to prevent actions from occurring during tests
  * flags to prevent files from being saved like user stats
  * logging levels for your application
* other things I haven't thought of.

# Post-Run Hook
The post-run hook is run after all tests are run and all output has been generated.  The post-run hook can access the pre-run script instance (if one was specified) via `gut.get_pre_run_script_instance()`.

The post-run hook could be useful in writing files used by CICD pipelines to verify the status fo the run.

# Summary Info
GUT tracks the results of all the scripts and tests that are run.  There is a Summary object that you can access via the `gut` variable.  Using this information you can take actions in the post-run hook.

Reading the documentation/code in [summary.gd](https://github.com/bitwes/Gut/blob/master/addons/gut/summary.gd) will get you the full details, but here are a few examples of how to get the summary data.

### Full Summary
```
# Returns GUT's summary.gd instance holding all the data about the run.
gut.get_summary()
```

### Counts
```
# This will return a dictionary containing the following counts:
#   passing = 0,
#   pending = 0,
#   failing = 0,
#   tests = 0,
#   scripts = 0
gut.get_summary().get_totals()
```

### All Scripts
```
# Returns and array of Summary.gd.TestScript objects that have detailed information
# about each script/inner class that was ran.  See summary.gd for more details.
gut.get_summary().get_scripts()
```
