## Installing from Download
Download and extract the zip from the [releases](https://github.com/bitwes/gut/releases) or from the [Godot Asset Library](https://godotengine.org/asset-library/asset/54).  

Extract the zip and place the `gut` directory into your `addons` directory in your project.  If you don't have an `addons` folder at the root of your project, then make one and THEN put the `gut` directory in there.

## Installing from in-editor Godot Asset Lib
1.  Click the AssetLib button at the top of the editor
1.  Search for "Gut"
1.  Click it.
1.  Click "Install".  This will kick off the download.
1.  Click the 2nd "Install" button that appears when the download finishes.  It will be in a little dialog at the bottom of the AssetLib window.
1.  Click the 3rd "Install" button.
1.  You did it!

## New Install Setup
### Activate
1.  From the menu choose Scene->Project Settings, click the plugins tab and activate Gut.

### Setup Your Project to Run Tests
The next few steps cover the suggested configuration.  Feel free to deviate where you see fit.

1.  Create directories to store your tests and test related code
	* `res://test`
	* `res://test/unit`
	* `res://test/integration`
1.  Create a scene that will use Gut to run your tests at `res://test/tests.tscn`
	* Add a Gut object the same way you would any other object.
	* Click "Add/Create Node"
	* type "Gut"
	* press enter.
1.  Configure Gut to find your tests.  Select it in the Scene Tree and set the following settings in the Inspector:
	* In the `Directory1` setting enter `res://test/unit`
	* In the `Directory2` setting enter `res://test/integration`

That's it.  The next step is to make some tests.

# Where to next?
* [Creating Tests](https://github.com/bitwes/Gut/wiki/Creating-Tests)<br/>
* [Gut Settings and Methods](https://github.com/bitwes/Gut/wiki/Gut-Settings-And-Methods)
