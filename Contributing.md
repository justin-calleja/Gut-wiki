This testing tool has tests of course.  All Gut related tests are found in the `test/unit` and `test/integration` directories.  Any enhancements or bug fixes should have a corresponding pull request with new tests.

For convenience, the `main.tscn` includes a handy "Run Gut Unit Tests" button that will kick off all the essential test scripts.

# Checklist for PRs
* New features must have unit tests.
* New asserts need to have a sample added to `res://test/samples/test_readme_examples.gd`.  These should illustrate fail/pass scenarios.
* If you are updating an assert, then changes should be made to `res://test/samples/test_readme_examples.gd` if applicable.
* Readme/wiki updates
  * If you want to take a stab at updating the wiki that is fine.  I think it's possible to make a PR against it but I haven't done that yet.  
  * You can also include some readme text in the PR and I'll add it.
  * You can also NOT include readme and I'll add it, but any typing you can save me is wonderful.
* CHANGES.md
  * I will credit you in the CHANGES.md (usually a "thanks" or "thanks to" or "done by" or "provided by" or "typed by" or "well done!" or something else clever if the spirit moves me).  If you have a handle you would like me to use (other than your github username) then let me know in the PR.  You don't need to make any changes to the CHANGES.md file, I'll handle that.  Depending on where I am in a release cycle changes may get incorporated into a new/existing branch.
