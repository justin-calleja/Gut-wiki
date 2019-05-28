As of version 6.7.0 the master branch is compatible with Godot 3.1 but there are some minor issues with a few features.

1.  `assert_is` will pass if an Inner Class name matches what you are comparing against, even if the Outer Class does not match.  I've created this issue with the engine:  https://github.com/godotengine/godot/issues/27111.  It looks like Godot's `is` method is a little broke.
1.  Exporting tests is broke in 3.1 but 3.1 has a workaround which is why I went ahead with a release.  Check the Exporting-Tests page for the workaround (it's actually easier than using the export functionality).
1.  There are a ton of warnings.  Some of these will be addressed, but there are a lot that cannot/should not be changed.
