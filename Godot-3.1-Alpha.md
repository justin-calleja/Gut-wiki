I've created a [3.1 branch](https://github.com/bitwes/Gut/tree/godot_3_1) that will be kept inline with master.  When I'm confident that 3.1 is solid then I'll combine master and 3.1.  I'll list any existing workarounds or major problems here, as well as in the Issues.

## Should be alright, probably
* Currently it looks like 3.1 defaults `user://` to the project's root on mac (which it didn't use to do).  This shouldn't be a big deal for your use of Gut, but it does cause some problems with the tests for Gut itself since I create files and delete them in `user://`.

## Big deals
* none yet

## Small deals
* none yet

## Not quite a big deal but for sure not a small one
* none yet