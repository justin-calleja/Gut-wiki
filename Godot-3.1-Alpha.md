I've created a [3.1 branch](https://github.com/bitwes/Gut/tree/godot_3_1) that will be kept inline with master.  When I'm confident that 3.1 is solid then I'll combine master and 3.1.  I'll list any existing workarounds or major problems here, as well as in the Issues.

If you find anything new or have any input, make/comment on an issue.

## Should be alright, probably
* It looks like 3.1 defaults `user://` to the project's root (on mac at least).  This shouldn't be a big deal for your use of Gut, but it does cause some problems with the tests for Gut itself since I create files and delete them in `user://`.<br/><br/>
If you are using `doubles`, this could result in a directory being created at the root of your project.  I delete the directory and create it each run so I can put generated doubles in there.  I name the folder `scripts`...just kidding.  But if you find a folder called `gut_temp_directory` at the root of your project then this is probably why.

## Big deals
* none yet

## Small deals
* none yet

## Not quite a big deal but for sure not a small one
* none yet