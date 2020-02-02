

[[https://raw.githubusercontent.com/wiki/bitwes/Gut/images/GutGui.png|alt=gutgui]]

1.  Output Box.
1.  List of Test Scripts.  Inner Classes are indented under scripts.
1.  Progress bars for all scripts and the current script.
1.  Log Level slider.  
1.  Previous Script (in list of scripts)
1.  Run the currently selected script and all scripts after it.  This can be especially useful when running on another device and some script in the middle of the list causes a crash.  To run the tests after the crash, just select that test in the list and click this button.  It will run that one and all the ones after.
1.  Next Script (in list of scripts)
1.  Run the currently selected script.  If an Inner Class is selected then just that class will be run.  If a Script is selected then the script and all of its Inner Classes will be run.
1.  Toggle display of List of Test Scripts
1.  The Hamburger button.  It shows some additional options.
1.  Continue button will be enabled if a call to `yield_before_teardown` occurs.  Click it to continue running tests.
1.  The title bar.  It has a maximize button, shows the current script, has a running tally on the left of the pass/fail count, shows the elapsed time.  Also you can drag it all about.

Also, in the bottom right corner, you can drag to resize the dialog.
