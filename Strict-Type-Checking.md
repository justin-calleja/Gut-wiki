##  <a name="strict"> Strict type checking
Gut performs type checks in the asserts when comparing two different types that would normally cause a runtime error.  With the type checking enabled (on be default) your test will fail instead of crashing.  Some types are ok to be compared such as Floats and Integers but if you attempt to compare a String with a Float your test will fail instead of blowing up.

You can disable this behavior if you like by calling `disable_strict_datatype_checks(true)` on your Gut node or by clicking the checkbox to "Disable Strict Datatype Checks" in the editor.
