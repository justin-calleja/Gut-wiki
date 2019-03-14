When you export your project, all the scripts get compiled and Gut cannot parse them anymore.  To address this Gut has some methods and settings that make it possible to run your tests on any device.

Exporting is only supported through a scene since there is no built-in way to run the tests in your exported game via the command line.  To that end, you will have to have a scene in your game that has a Gut node (See the Setup section on the Install page).  I'm going to assume the node name is `$Gut`.  

## Configuring Gut to Export
Select the Gut node and set the and set the `Export Path` setting in the Inspector.  This must be a file in the `res://` directory.  I've been using `res://test/exported_tests.cfg`.  This way the file won't be included when you create a production export which, of course, excludes the `test` directory from the build.

You must also be sure that your project is configured to export files with the extension you give in the setting.
* Click Project->Export
* Select the Preset to configure.
* Under the Resources tab make sure that `*.cfg` is in the list (or whatever extension you used when setting `Export Path`).
```
*.txt, *.cfg
```

## Auto Export/Import
Gut has a couple of methods that should allow you to automatically export and import your settings in most cases.  They are:
* `export_if_tests_found()`
* `import_tests_if_none_found()`

When you add these methods to the `_ready()` of your scene then Gut should take the proper action based on whether you are running from the editor or on some other device.
```
func _ready():
  # always put them in this order
  $Gut.export_if_tests_found()
  $Gut.import_tests_if_none_found()
```
Both methods will either print an error if something went wrong or a complete list of everything they exported/imported and the name of the file it used.

Both of these methods __require__ that the `Export Path` property has been set.  The `_ready` method will be called after Gut has loaded up all the directories you have configured in the editor.  If Gut has found tests it was able to parse then `export_if_tests_found` will write them to the `Export Path`.  If it cannot find any tests then the `import_tests_if_none_found` line will load the file at `Export Path` and all your tests will be ready to go.

__NOTE__:  It is recommended that you do not configure your scene to `Run On Load` when you are exporting your tests.  If a test causes a crash on a device, then you won't be able to use the GUI to run other tests, it will just run until it crashes.

## Manually Exporting/Importing
Gut also has methods that you can use to have full control over when and where you export/import tests.
* `set/get_export_path(...)`
* `export_tests(path=export_path)`
* `import_tests(path=export_path)`

As you probably inferred, `export_tests` and `import_tests` will use any path you give them or the value set by `set_export_path`.
