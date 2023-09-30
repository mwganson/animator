# Animator Macro
FreeCAD Macro to animate a model.  Run the macro to create an Animator feature python object.  Select the available properties from a list and double click the object to start the animation.  Selected properties will be incremented (or decremented if a negative Step value is chosen) each time through the animation loop.

## Toolbar icon
<img src="Animator.svg" alt="toolbar icon"><a href="Animator.svg">Download</a> the toolbar icon.

## Installation
Install with the Addon Manager.  On first run it will offer to create a file (animator.py).  This new file is needed in order for FreeCAD to be able to load the class definitions needed by the Animator objects when opening a file containing one of the saved objects.  When you uninstall Animator you must manually remove the animatory.py file.

## Properties
Like all feature python objects the Animator objects are controlled largely by editing their properties in the property view.  Some of the boolean properties act as triggers for commands.  When toggling a trigger from False to True the command is executed and the trigger sets itself back to False, awaiting the next trigger.
## Animated Gif section
New beginning with version 0.2023.09.13.  These properties control the optional creation of an animated gif during the animation.  During each loop a screen capture is done of the 3d view and placed into a png file named Frame_NNN.png (where NNN is the current frame).  For example, if you have a 3-frame animation then you will have Frame_001.png, Frame_002.png, and Frame_003.png in the gif folder, in addition to the animated gif file.  These frames are deleted prior to running the next animation if Make Gif is set to True (or else they would be incoporated into the new gif).  If the animated gif already exists, it, too, will get replaced.  Use a different Animated Gif Filename if you want to keep it or rename it manually or move it to a new location or set a new location for the animated gif to be placed in.  Most of the time you will probably want to replace the gif since making this is likely to be an interative, trial and error process in most cases.
### Animated Gif Filename (string)
The name to give to the animated gif that gets created.  If there already exists a file with the same name in the folder it will be replaced.
### BGColor (string)
The color of the background to put into the animated gif.  Default is "Transparent".  This is the name of the color, so you're free to try different colors here, such as "green", "blue", "chartreuse", etc.
### Fit All Each Frame (boolean)
Default: True.  Sends Gui.sendMsgToActiveView("ViewFit") just before taking the screen shot for each frame.  This is the Std_ViewFitAll (shortcut: V,F) in the FreeCAD Gui.  It fits the model to the screen.
### Gif Frame Rate (integer)
Default: 20 milliseconds.  How many milliseconds to show each frame for before moving to the next frame.  Lowest accepted value is 1 millisecond.  Highest is a million seconds, not that you would likely ever need that frame rate.
### Gif Height (integer)
Default: 480.  This is the height in pixels.
### Gif Width (integer)
Default 640. This is the width in pixels.
### Make Gif (boolean)
Default:False -- do not make the animated gif.
### Path To FFMPEG (file)
Set this to point to the ffmpeg executable binary wherever it happens to be on your system.  Once you set its location it gets remembered the next time you create a new Animator object, so you should only need to set it once.  The value gets stored in user preferences:/plugins/Animator_Macro.  Tools menu -> edit parameters -> plugins -> Animator_Macro.  If you uninstall the macro you can deleted these parameters, too, or just leave them there.
### Path To Gif (folder)
Set this to point to the folder you want to save the animation frame files and final .gif file into.  If you leave it blank it will create a subfolder in your Macros folder called Animator and inside that one another subfolder called screen_grabs, which is where you will find the frame pngs and the animated gif.  Note: the frame pngs are deleted each time a new animated gif is produced and if the new animated gif is the same name as an existing gif it will get replaced.  This, like the Path To FFMPEG parameter is also saved in parameters.
## Animator section
### Frames (integer)
Default: 100.  This is how many frames / iterations there will be in the animation loop.  Internally a counter begins at 1 and is incremented by 1 each time through the loop.   The animation ends with the counter reaches 100 or if the user double clicks the Animator object or toggles Stop Animation property to True.
### Initial Delay (float constraint)
Default: 0 milliseconds.  This is the initial delay (time between triggering the animation to begin and the animation beginning).  Default is 0, so the animation starts immediately.  (Actually, there is a tiny delay of 50/1000 seconds in order to avoid some error messages appearing related to recomputes happening while properties are still being toggled.)  Use this if there are some things you want to do before the animation starts, such as setting up a screen capture.  This property is in milliseconds, so you need to use, for example, 5000 for a 5 second delay.
### Refresh (boolean toggle)
Default: False.  Toggle this to True in order to execute the Refresh command.  Refresh will find all objects in the current document and list their supported properties in the VariableNNN enumerations.  Supported properties are generally properties that can be modified by adding or subtracting a numerical value to them.  They must also be visible and not readonly with the exception of Spreadsheet properties, which is how we gain access to the aliases, as they are stored as hidden, readonly properties of the Spreadsheet objects.  (A note on spreadsheets: any expressions used in a cell that is being animated are going to be lost and not recovered when the animation is complete.  For all other property types, including named constraints in sketches, the expressions are restored upon completion of the animation.)
### Show Progress (boolean)
Default: True.  If True progress of the animation is shown in the Description / Label2 column of the feature python Animator object.  It is in the form of counter/Frames, for example, 325/400 if the counter is at 325 and there are 400 Frames in the animation.  Toggling this to False might marginally speed things up if you are finding they are going too slowly.
### Sleep (float)
Default: 0.  The amount of time (in seconds) to sleep during each Frame.  Normally you will want to leave this at zero, but if you want to slow down the animation this is a way to do it.  For example, if you want to add 1/4 second to each frame, then use 0.250 as the Sleep property.  (Note: this unit differs from the Initial Delay, which is in milliseconds because one uses Qt the other uses python.)
### Start Animating (boolean trigger)
Default: False.  Toggle this to True to begin the animation once you have selected a property (or multiple properties) to animate.  You can also double click on the Animator object in the tree view to Start Animating and to Stop Animating.
### Stop Animating (boolean trigger)
Default: False.  Toggle this to True to stop the animation or double click the Animator object in the tree view.
### Variable Count (integer constraint)
Default: 3.  Minimum: 0.  Maximum: 100.  This is how many variables to have available for animating properties.  One property per Variable.  (But you can also link multiple properties to one another with FreeCAD expressions, if you like.)  If you change this variable some VariableNNN properties will be created or removed from the Animator object.  If more are created, existing VariableNNN properties will retain their current settings.  Newly created VariableNNN properties will have default values.  (Note: 100 is the maximum, but if you find you need more, you can add more as an expression.  Press = key and enter the number you want in the dialog.)
## Animator Variables section
In this section are the variables.  Each variable will have 4 associated properties: the property to animate, whether to skip some frames, the initial value for the property to be animated, the step (increment/decrement) to be applied each iteration through the frame loop.
### VariableNNN (enumeration)
NNN is a placeholder for a 3-digit number assigned to each variable, beginning with 001.  The enumeration is a list of strings, each in the form of objectname.property.  In some cases a property can have subproperties.  For example, placement properties will have something like: Body.Placement.Base.x. For named constraints in a sketch, an example would be: Sketch002.Constraints.Height.  "Select Property" is the first item in the enumeration, as a prompt for you to select a property.  If the selection is "Select Property", then that VariableNNN is unused during the animation loop.  If all VariableNNN properties are on "Select Property", then an error is displayed and there is no animation loop.  If you cannot find a property in the list it might mean the property or object it belongs to was created after the Animator object was created.  Toggle the Refresh trigger property to True to refresh the property list.  If that doesn't work, then it might mean the property type is unsupported.  You can try adding it to the Supported property in the Settings Advanced section.  It might also be the property is readonly or hidden.  This can be remedied in some cases by using obj.setEditorMode("property name", 0) in the python console where obj is the name of the object an dproperty name is the name of the property.
### VariableNNN Nth (integer)
Default: 1.  This means increment the property every Frame through the loop.  A value of 2 would mean increment the property every 2nd Frame.  Note: when an increment is done it is a cumulative increment.  The formula is Start value + counter * Step.  The upshot of this is if you have Step = 1 and Nth = 3, then every 3rd frame the property gets incremented by 3.  If this is not what you intend then use a different Step value, for example Step = 0.33333 so that every 3rd Frame the increment is 3 * 0.33333 or about 0.9999999.
### VariableNNN Start (float)
Default: 0.  The starting value for an animated property.  When the animation begins the property is set to this value at the beginning of the loop.  Note: properties are not reset to their original values after the animation is complete.  The objects remain at their position occupied in the final Frame except for properties that have been set with expressions.  For example, a Cylinder Height property might be set with an expression = 2 * Radius so that its Height is always 2 * its Radius.  If you elect to animate the Height property it will animate with the values it is set to by the Animator object during the loop.  Then at the end of the loop it reverts to its former Height of 2 * Radius, whatever the current Radius is.  If the Height is not set with an expression, then it retains the Height it had in the final Frame of the loop.  Set properties with expressions if you want them to revert.  An expression can also just be a number, like by pressing = and then entering 10 in the dialog.
### VariableNNN Step (float)
Default: 1.0.  The amount by which to increment or decrement the animated property each Frame.  Use a negative value if you wish to decrement.  For example, to rotate an object from 45 degrees to 15 degrees you would set the Start property to 45, the Step property to -1, and the Frames property to 30.  Alternatively, you could set Step to -0.5 and Frames to 60 to do this in 0.5 degree increments.
## Assembler section
These are related to various assemblers you might be using.
### A2Plus Solve (boolean)
### Asm3 Solve (boolean)
### Asm4 Solve (boolean)
Executes the Assembly workbench solver each step through the loop if True.  Be sure to have opened the assembler workbench at least once during the sesssion so the command is loaded into FreeCAD or else the macro will fail.
### A2PlusConstraints (stringlist)
List of found A2Plus constraints in the current document.  You might need to toggle Refresh to update from time to time if new constraints are added.  Put a python style comment character pound (#) as first character of line to not solve that constraint (or you may also just delete it from the list).  New constraints may be typed in, but ensure the spelling is exactly correct.  Note: [] empty list means solve all constraints.
### A2PlusRefreshConstraints (bool)
If True, when Refresh is toggled, then the A2Plus constraints will also be updated.  (Warning: this will overwrite any python style comments on any of the lines.)  Set this to False if you want to refresh the other variables in the document, but not the A2Plus Constraints.
## Run Macro
Here we have properties related to the feature of allowing to run an arbitrary macro file (or code string in the Macro String property) each frame of the animation.  This is obviously a security risk, so the user is required to give permission each time the animation is started.  This permission dialog can be bypassed by starting the animation from the python console and passing a value of True to the function.  Select the animator icon in the tree, press Ctrl+Shift+P.  Then in the python console enter<br/>
<br/>
<pre>
obj.Proxy.startAnimating(True)
</pre><br/>
<br/>
Note: Even bypassing it in this manner still requires user permission in the dialog at least 1 time per session.

The file to be run is the file path and name held in the Macro File property.  It must contain valid python code.  It can have any extension, such as .py or .FCMacro or .txt.  The file is loaded into memory when the animation begins, then this in-memory string is what is executed each time through the loop.  For each frame the macro is being run anew, so it can't store local variables in memory as you normally would.  For persistent variables put them in the FreeCAD namespace.  Example:
<pre>
if not hasattr(FreeCAD, "MyVariable"):
    FreeCAD.MyVariable = 1
FreeCAD.MyVariable += 1
print (FreeCAD.MyVariable)
</pre>

Now you have access to FreeCAD.MyVariable each time through the loop.

There is a variable available to the macro file called AnimatorLastFrame.  This is a boolean variable set to True when the macro is being called in the final frame of the animation.  It need not be defined in the macro code before being accessed:

<pre>
if AnimatorLastFrame:
    print("This is the final frame in the animation.")
</pre>

Assorted variables pre-defined for your use in your python code:<br/>

##### FreeCAD
##### App
##### FreeCADGui
##### Gui
##### AnimatorLastFrame (boolean, True = in last frame of animation)
##### doc
##### Animator
##### fp
##### con (FreeCAD.Console -- e.g. con.PrintMessage("your message\n"))
##### fp.CurrentFrame -- current frame of the animation loop (starts with 1)
##### props -- list of strings of the properties in the VariablesNNN enumerations.  For example, if Variable001 = "Sketch.Constraints.Width" then props[0] = "Constraints.Width".
##### objs -- list of objects referenced in the VariablesNNN properties. For example, if Variable001 = Cylinder.Height, then objs[0] = the Cylinder document object.
##### setExpression(idx,expr) -- sets the value of objs[idx] property props[idx] to expr.  Example, if Variable002 = "Cylinder.Radius", then setExpression(1,"2.5") sets Cylinder.Radius to the expression "2.5".  Other example: setExpression(1, "Box.Height")  The parameter idx is the index into the props and objs lists, which are 0-indexed.
##### getValue(idx) -- gets the value of the props[idx] at this point in the animation loop.  If you want to get and set values for a property easily in this manner, then select them for the various VariableNNN properties and set the VariableNNN Step property to 0.  That way they're not being animated in the loop, but your code has easy access to them for setting and retrieving values.
##### setStep(idx,val) -- sets the step value to val. Example: setStep(0,3) would set Variable001 Step = 3.0.  Note: if step = 0, then the property is not animated.  Note2: the animation "catches up" to where it would have been had step value been 3 the whole time in the previous example.  The property is set as start value + step value * counter (where counter is the number of the current frame).  It is better to set the property directly rather than using step unless this behavior is what you want.  For example, if you want to set Box.Height to its current value + 3 each time in the loop after having paused the loop for a time by setting step = 0, then set the box object's Height property directly: objs[0].setExpression("Height",str(objs[0].Height+3))

### Run Macro String (boolean)
Default: True.  Set to False if you don't want to run the string in Macro String each time through the loop.  (Note: The string is not run if it contains the default values, which are all just comments, anyway.)
### Macro String (String list)
Default: Some commented lines to give some quick help for getting started.  Add your own code to the bottom of the list or delete the comments and substitute your own code.  Each item in the list is its own python line of code, so be sure to add spaces to the front where indenting is needed for that line.  You can copy/paste code into the string list editor.
### Run Macro (boolean)
Default: True.  Set to False if you don't want the macro file to be executed each frame during the loop.
### Macro File (File)
This is the file to be executed each frame of the animation.
## Settings Advanced section
Here we have a few advanced settings you will not normally need to change.
### Blacklisted Objects (string list)
These are the objects that will not appear in the VariableNNN property lists.  By default, we exclude the Origin, Axis, and Plane objects in Part Design bodies.  This list is editable if you want to exclude other types.  For example, to exclude Part Design Body objects you could add "PartDesign::Body" to the Blacklisted Objects property.  This is the object's TypeId property.  To find an object's TypeId you can select it in the tree and press Ctrl+Shift+P to send the obj variable to the python console.  Then in the python console enter obj.TypeId.
### Supported (string list)
These are the supported properties.  You may add another property type to the list to add support for it, but if the new property type has subproperties, then it will not function correctly.  Ping me on the forum <TheMarkster> and I will see about adding the new support for the new property type for you.  Other than that, the new property should (hopefully) work just by adding it.  Just remember it must be something that will accept incrementing by a floating point value.  You can test this in the python console by entering:  obj.setExpression("PropertyName","0.1") where obj is the object containing the property, "PropertyName" is the name of the property, and "0.1" is the value you wish to set it to.  If this works, then adding the property type to the Supported list should also work.  To determine the property type, right click on the property and select Show All from the context menu.  Then hover your mouse over the property name to see the tooltip showing it's property name, usually something like "App::PropertyFloat."
## ChangeLog
* 0.2023.09.30b<br/>
** fix issue with ReverseCam property (thanks to edwilliams16)
* 0.2023.09.30<br/>
** change Camera class to use property setters and getters
** add Placment property to Camera class
** add Camera group to Animator properties
** add CamPlacementObject property and ReverseCam boolean (not currently working)
* 0.2023.09.16<br/>
** fix bug where macro wasn't running even when giving permission
* 0.2023.09.13<br/>
** add ability to make animated gifs from animation (requires FFMPEG binary)
* 0.2023.09.12d<br/>
** add setStep() function
** check step each time through animation loop in case user has changed it
* 0.2023.09.12<br/>
** add some more variables to context of macro file and new macro string<br/>
** add ability to run a macro stringlist property for quick and dirty macros that don't need their own file<br/>
** fix an issue that broke existing models in previous update by attempting to reference a new property that doesn't exist in the old objects<br/>
** fix a bug where document wasn't getting completely restored after animation completed<br/>
** Add new items to context menu
* 0.2023.09.03b<br/>
** add AnimatorLastFrame boolean variable to context of macro file
* 0.2023.09.03<br/>
** fix bug where if user forgot to select a property to animate the animation no longer worked even after selecting a property
** add feature to run arbitrary macro file during each frame of the animation
* 0.2022.06.04<br/>
** accept PR from m1pro to add extra call to process events at start of animation loop <br/>
** fixes issue with link branch <br/>
* 0.2022.03.07<br/>
** Add support for A2Plus constraints, individually disabling/enabling during A2PlusSolve<br/>
** Add support for Yaw, Pitch, and Roll placement.rotation properties<br/>
** Add Undo support to put model back to where it was when animation first began<br/>
* 0.2022.01.10 == Always use Placement and Attachment Offset properties even when hidden
* 0.2021.10.29 == Support for new property type: "Rotation"
* 0.2021.10.28 == initial upload
