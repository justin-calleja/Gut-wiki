A partial double is basically the inverse of a doubled object.  It creates an object that acts exactly the same way as its source but has hooks that allows us to stub or spy or any method.  

The `partial_double` method will create a partial double "class" which you can instance.  You can partially double scripts, packed scenes, and inner classes.  It works the same way as `double` works, so read up on that and then apply all that new knowledge to `partial_double`.

Under the covers, whether you call `double` or `partial_double`, GUT makes the same thing.  If you call `partial_double` though, it will setup the object's methods to be stubbed `to_call_super` instead of not being stubbed at all.

After you have your partial double, you can stub methods to return values instead of doing what they normally do.  You can also spy on any of the methods.
