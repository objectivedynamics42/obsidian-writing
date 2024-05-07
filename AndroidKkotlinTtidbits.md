Things I've discovered about Android and Kotlin development

# Layouts
## DP stands for Device Independent Pixels
Irrespective of the device being used there are 160 device independent pixels per inch
## Constaint layouts
* Views should always have a vertical and horitontal constraint or the view will float to 0,0
* R is a generated class
* Pre-built layout classes can be found in android.R.layout
* setContentView - generally called from onCreate - determines which layout an activity should load

## Options Menus
* These allow us to provide actions for  an activity  - available in the app bar under the _action overflow_ i.e. the _hamburger_ menu
* Options menus are defined in a _menu resource_
* To handle them we need to override _onCreateOptionsMenu_ 
* Menu options have two key characteristics: the display text and the menu item's id
* The ID must be unique within each menu
* In _onCreateOptionsMenu_ we need to check the id of the selected item to determine the action that the activity needs to take
### Gotchas
When I first tried to add a menu bar it wasn't visible and none of the options menu overrides were being invoked. It turns out that the issue was lurking in _app/src/main/res/values/themes.xml_ where the _empty views activity_ code generator had specified _Theme.Material3.DayNight.NoActionBar_ as the _base application theme_:
```
 <style name="Base.Theme.Nutribase" parent="Theme.Material3.DayNight.NoActionBar">
```

```
<style name="Base.Theme.Nutribase" parent="Theme.Material3.DayNight">
```

# Classes
* Can provide an override by selecting the class name in the code and clicking _Code | Override Methods..._

## Data classes
* Eligible classes must have a primary consructor that only declares properties using e.g. _val_ or _var_

## Objects and the Singleton Pattern
* Kotlin supports the concept of an _object_. We've already seen this used in NutriBase where an _object_ was implemented as a _companion_ in the _FoodDetailsActivity_ and _TaggedFoodListActivity_ classes. Object classes are similar to static classes in other languages. Companion objects are similar to _friends_ in _C++_

# Interfaces
* Kotlin has the concept of a _SAM_ (Single Access Method) interface. A SAM interface may also be called a _functional interface_. SAM interfaces mandate only a single method in which case it can be implemented by a _Lambda_

# Menus
## Action Overflow
Handy - I implemented a demo in my _pluralsight_experiments_ nutribase branch. May not be as easily discoverable as App Bar thingies though
## App Bar
### ShowAsAction Property
Values:
* *ifRoom* - as it says on the tin. If there's no room for an item AIUI it's then moved to the Action Overflow
* *always* - can cause crowding. Official guidelines say that you should have no more than two menu items should have this set to true but it might be wiser to be more restrictive than that
* _withText_ text to support the icon

### Icons
* Can be added by including a new vector asset into the project
* It'll be stored in the _drawable_ folder
* Include the icon by selecting the menu resource in the designer and then clicking the button to the right of the _icon_ input
* Text can be shown on the action bar if there's room - e.g. if the device is oriented in landscape. This can be achieved by clicking the icon to the left of the menu item's _showAsAction_ property and choosing _withText_. 
### Modifying the state of a menu at runtime
* Override _onPrepareOptionsMenu_
* Invoked _invalidateOptionsMenu_ to notify the framework that we've changed the menu

# Nullable
* ?. 
  The _Safe Call_ operator. if the reference is non null, the operator returns the object. Otherwise it returns a null reference
* ?: 
  The _Elvis_ operator. if the value is non null, the operator returns the value. Otherwise it returns an alternative value

# Activity Lifecycle
## Lifecycle methods
* onCreate
* onRestart - see below too
* onStart - visible
* onResume - foreground
* onPause - background
* onStop - not visible
* onRestart - called after being fully obscured
* onDestroy - shutting down
## Persisting Data
Most Android applications automatically save any changes when you leave an activity. Official guidelines
* Override onPause
## Tasks
Android can destroy any activity that's not in use in order to free resources - even when an activity is part of an app's current workflow. When the user presses *back*, they may be returning to an activity that's been destroyed.
* Managed as a stack
* Moving forward, activities are created and added to a *Task*
* Moving backward, activities are destroyed and removed from that task
**Check:** It looks like the process of managing tasks and activities is automatic.
## Activity Instance State

Activities can be destroyed and re-created in interesting scenarios. One is when changing orientation!
* onSaveInstanceState - only called when the system is destroying an activity with the intention of potentially recreating the activity instance later
* onCreate is passed a bundle that can be used to rebuild previously persisted state
* Don't store everything though. Just the stuff that's changed since the activity was initially created because when our activity is recreated we'll still have access to the activity intent
**Check:** the last point in particular will require some reading I think
