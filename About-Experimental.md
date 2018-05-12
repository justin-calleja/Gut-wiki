Features marked as "Experimental" might not be fully implemented or could have an early implementation that may need to be changed later.  Use at your own risk.  These features will not be removed unless they absolutely need to be but they have a greater chance of have breaking changes later down the road.

Feedback about these features is __greatly__ appreciated.

# Current Experimental Features:
The current set of experimental features revolve around creating test doubles for objects and then using them.  
* Doubling
* Stubbing
* Spies

Doubling has proven to be a bit of a beast.  There's a lot of "dynamic" programming that has been done to make them work.  GDScripts are created dynamically at runtime and then loaded to provide the doubles.  There's a lot that can go wrong but I think I've got a good base to work from now.  

Once you double a script you can stub methods to return certain values.  You can also "spy" on the double and make assertions on which functions were called and what parameter values it was sent.

It isn't perfect yet.  There are inheritance issues that need to be sorted out.  Currently you only get doubled functions for anything that your script or its script parents have defined.  It does not double Godot built-in methods in Super Classes.  You can usually work around this b/c they retain their original implementation and you can just make normal asserts in most cases.  It can get tricky if you overload some accessors but not others.  For instance, if you overload `set_position` in your script, it will get doubled but `get_position` will not.

I'm really excited about these features but they have a little ways to go still.  Any feedback will be VERY helpful.
