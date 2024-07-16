# 2024-06-30 (Sunday)
## Room CodeLab
I temporarily skipped ```steps 11 and 12```of the [android-room-with-a-view-kotlin](https://developer.android.com/codelabs/android-room-with-a-view-kotlin#0) codelab and completed ```step13```. On second thoughts though I should return and add the missing steps before continuing onto ```step 14```. It'll mean that my app is essentially a clone of the codelab app but it also means I'm less likely to miss something that'll cause a  problem

# 2024-07-01 (Wednesday)
I switched to adding Room support to Nutribase as I already have a framework in place for the app. It didn't seem to be worth building one just as a demo.
As it stands I've added the following but they are not currently hooked up:
* com.objectivedynamics.nutribase.database.AppDatabase
* com.objectivedynamics.nutribase.entities.Tag
* com.objectivedynamics.nutribase.entities.TagDao
* com.objectivedynamics.nutribase.NutribaseApp
## Next steps
Fix the exception that's occurring when the *NutribaseApp* class invokes AppDatabase.getDatabase. The exception is: 
*java.lang.RuntimeException: Cannot find implementation for com.objectivedynamics.nutribase.database.AppDatabase. AppDatabase_Impl does not exist*
* It would appear that for some reason the database implementations aren't being generated
* Note that the Room261ClientAndroidApi28 application does have generated code

# 2024-07-04 - Thursday
I'll get to looking into the exception but first I need to try to resolve some other issues. During the build I'm seeing some warnings about using deprecated facilities
```
Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.
```
To try to resolve these issues I added  ```org.gradle.warning.mode=all``` to my ```nutribase\gradle.properties``` file

After adding that I performed a clean and got this:
```
The org.gradle.api.plugins.Convention type has been deprecated. This is scheduled to be removed in Gradle 9.0. Consult the upgrading guide for further information 
```
Here's the [upgrading guide](https://docs.gradle.org/8.7/userguide/upgrading_version_8.html#deprecated_access_to_conventions)

# 2024-07-06 - Saturday
I rolled back my nutribase changes to try to get an understanding of when and why I start to get the warnings I was seeing on Wednesday.
Even after rolling back to the last commits that were pushed to origin/main, I was still seeing the warning.
Possibilities are:
* [update gradle](https://github.com/objectivedynamics42/nutribase/commit/a357930bf541a3e6c59c0fc58e9bd803c08c7580 "update gradle")
* [upgrade gradle wrapper](https://github.com/objectivedynamics42/nutribase/commit/6e59b9e31f476304af8a1589e109ac8367ef1be8 "upgrade gradle wrapper")
* [added a dependency on Room 2.6.1](https://github.com/objectivedynamics42/nutribase/commit/2dddd891fce79fe897ac750a6690d9f1c56830fc "added a dependency on Room 2.6.1")
* [added initial room classes - work still to be done](https://github.com/objectivedynamics42/nutribase/commit/5d6db5fb471b3d00c6dd27a32b390ccabb03afa5 "added initial room classes - work still to be done")
* [added an application class](https://github.com/objectivedynamics42/nutribase/commit/e13c97e70a87d5ade9e9ea410f10500029a33288 "added an application class")
Try performing the above in stages on a new *out of the box* app and see where it gets me

## Gradle Properties
Courtesy of [Add '--warning-mode all' command line argument in Android Studio](https://stackoverflow.com/questions/54619731/add-warning-mode-all-command-line-argument-in-android-studio) question on Stack Overflow:
Add ```org.gradle.warning.mode=all``` to the ```gradle.proprties``` file in order to get more detailed build reports

I cleaned the project:
```
> Configure project :app
The org.gradle.api.plugins.Convention type has been deprecated. This is scheduled to be removed in Gradle 9.0. Consult the upgrading guide for further information: https://docs.gradle.org/8.7/userguide/upgrading_version_8.html#deprecated_access_to_conventions
```

### Disabling Plugins
Go to ```File | Settings | Plugins | Installed``` and uncheck the plugins one at a time. This will probably require a restart of Android Studio each time

I disabled the following with no obvious difference in the warning
* Smali
That prevented Android Studio from restarting!
I had to delete ```C:\Users\mike\AppData\Roaming\Google\AndroidStudio2024.1\disabled_plugins.txt``` in order to get it to restart

* Code Coverage for Java
* HTML and XML
* Keymap
* Local AI/ML Tools
* Plugin Development
* Version Controls

## Reinstalling Android Studio
I was advised (by Gemini!) that I'd be able to find the culprit of the dependency issue by issuing a ```./gradlew dependencies``` command but that just opened up another can of worms:
```
$ ./gradlew dependencies Error: could not open `C:\Program Files\Android\Android Studio\jbr\lib\jvm.cfg'
```

I decided it was time cut my losses and to reinstall Android Studio in an attempt to at least get a consistent Java environment.
It didn't resolve the issue but at least after the reinstall at least I was able to run the ```./gradlew``` command

## Upgrading Gradle
In ```GradleWarningSpike\gradle\libs.versions.toml``` I updated the value of ```distributionUrl``` to 
```https\://services.gradle.org/distributions/gradle-8.8-bin.zip```

I then checked to see that it had been effective:
```
$ ./gradlew -version

------------------------------------------------------------
Gradle 8.8
------------------------------------------------------------

Build time:   2024-05-31 21:46:56 UTC
Revision:     4bd1b3d3fc3f31db5a26eecb416a165b8cc36082

Kotlin:       1.9.22
Groovy:       3.0.21
Ant:          Apache Ant(TM) version 1.10.13 compiled on January 4 2023
JVM:          17.0.10 (JetBrains s.r.o. 17.0.10+0--11609105)
OS:           Windows 10 10.0 amd64
```
## Resolving the deprecated dependency issue using gradlew
Contrary to what I'd been told by Gemini
```./gradlew dependencies```  didn't git me anything. However, another ```gradlew``` command did:
```
$ ./gradlew build --stacktrace
```

The above produced an exception in the console but it also generated this html report:
```
GradleWarningSpike/app/build/reports/lint-results-debug.html
```

Under a section titled ```Obsolete Gradle Dependency``` it highlighted a number of entries in  ```
gradle/libs.versions.toml``` that should be upgraded:
* ```kotlin``` from 1.9.0 to 2.0.0
* ```navigationFragmentKtx``` from 2.6.0 to 2.7.7
* ```navigationUiKtx``` from 2.6.0 to 2.7.7

I applied the above changes, cleaned and rebuilt the project and the deprecated dependency issue was gone!

## Applying the changes to nutribase
I upgraded (and verified at the command line) the Gradle wrapper from 8.7 to 8.8:
```
$ ./gradlew -version

------------------------------------------------------------
Gradle 8.8
------------------------------------------------------------

Build time:   2024-05-31 21:46:56 UTC
Revision:     4bd1b3d3fc3f31db5a26eecb416a165b8cc36082

Kotlin:       1.9.22
Groovy:       3.0.21
Ant:          Apache Ant(TM) version 1.10.13 compiled on January 4 2023
JVM:          17.0.10 (JetBrains s.r.o. 17.0.10+0--11609105)
OS:           Windows 10 10.0 amd64
```

I then accepted all the update suggestions in ```gradle/libs.versions.toml```,  which were:
* kotlin
* coreKtx
* junitVersion
* espressoCore
* appcompat
* material
* activity

The *gift that just keeps giving* then came up with this one:
```e: file:///C:/Users/mike/AndroidStudioProjects/nutribase/app/build.gradle.kts:1:57: Unresolved reference: archivesName```

I commented out the two offending lines in the app's ```build.gradle.kts``` file and then cleaned and rebuilt the project and **the deprecated dependencies was finally gone!!!**

All that remained was to install a *Pixel 7* with *Android 14 Upside Down Cake (API 34)* and to  run *nutribase*.

**It worked!!!**

Now to resolve the KSP/Room issue that I started with!

## Adding KSP back into the project
I added KSP version 2.0.0, rebuilt and launched in the debugger:
```
Unable to create application com.objectivedynamics.nutribase.NutribaseApp: java.lang.RuntimeException: Cannot find implementation for com.objectivedynamics.nutribase.database.AppDatabase. AppDatabase_Impl does not exist
```

So we're back where we started. KSP isn't generating the necessary implementation files.

## KSP not needed in the build.gradle.kts files
This came from Gemini who said that with versions of Room > 2.6.0 it's not needed as Kotlin takes care of everything. I'm not sure that I believe it but at least for now I've commented the references to ksp

## Improved gradle logging
Launch from the shell:
```
$ ./gradlew clean build --info
```
# 2024-07-07 - Sunday
## What version of Kotlin is in use?
```
	println("Kotlin version: ${KotlinVersion.CURRENT}")
	
```

This is in line with the version specified in  ```libs.versions.toml```:

```
    kotlin = "2.0.0"
```

## Upgrading the Gradle Plugin
* In this case From 7.1.0 to 7.4.2
### Files changed
* build.gradle
  ext.kotlin_version = '1.6.21'
  classpath 'com.android.tools.build:gradle:7.4.2'
* gradle/wrapper/gradle-wrapper.properties
  distributionUrl=https\://services.gradle.org/distributions/gradle-7.5-all.zip
## Android Architecture Guide
Buried in the ```codelab-android-room-with-a-view-kotlin``` source code was this link to the [Architecture Guide]( https://developer.android.com/topic/libraries/architecture/guide.html)


## Back To The Story
See the [migrate-to-ksp](https://developer.android.com/build/migrate-to-ksp) articled on [developer.android.com](https://developer.android.com)
This worked. At least in that code was finally generated. See the KSP differences in the build.gradle.kts files and try to roll back the source code changes around the database class. They are causing build errors

# 2024-07-08 - Monday
## RoomSpike
I didn't like the idea of trialling Room/ksp in nutribase so I created a spike for the work: 
* I created a new Android Studio Empty Views app
* I initialised a local git repo for it
* I added the bare minimum code and config to demonstrate ksp/Room functionality
* Once I'd committed the changes I created a [new RoomSpike project](https://github.com/objectivedynamics42/RoomSpike) on github
* I pushed my recent commits to the github repo:
```
$ git remote add origin https://github.com/objectivedynamics42/RoomSpike.git
$ git branch -M main
$ git push -u origin main
Enumerating objects: 110, done.
Counting objects: 100% (110/110), done.
Delta compression using up to 4 threads
Compressing objects: 100% (89/89), done.
Writing objects: 100% (110/110), 104.15 KiB | 3.16 MiB/s, done.
Total 110 (delta 11), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (11/11), done.
To https://github.com/objectivedynamics42/RoomSpike.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

# 2024-07-09 - Tuesday
I ran into a bit of a dead end when I tried to hook up the database as it needed a ViewAdapter and the Empty Views application doesn't have one.

# 2024-07-11 - Thursday
## Ajay's spreadsheet
### Overview
There are 3 visible tabs:
* Training Plan
* Logbook
* Exercise Database
#### Training Plan Tab
* Program Focus - This currently has 6 weeks worth of data but they look to be hard coded and so there's likely to be little work in extending these
* Main area. This has sections for Lower body, Upper Body, Lower Body 2 and Full. Each area has columns for Order, Target Muscle, Exercise and Notes. It looks like **the Order column is hard coded but the Target Muscle and Exercise columns aren't** so they'll need some looking at
* 
#### Logbook Tab
AAA
#### Exercise Database Tab
This table consists of 126 rows, each row containing columns for Id, Muscle, Exercise, Notes and Demo. With the exception if the id column which is a simple counter, all the other cells are hard coded
### Training Tab
#### Target Muscle
This is driven by a formula.
Take the Machine Shoulder Press exercise in the section for upper body. It lives in cells D34:E35 and the Target Muscle field is currently showing the value: Delts.
This is arrived at using the following formula:
```
=IF(D34="",,vlookup(D34,{'Exercise Database'!D:D,'Exercise Database'!C:C},2,false))
```
#### Exercise

  

The dropdowns 
```
='Exercise Database'!$D$3:$D
```

#### Training Plan Data Validation 
##### Apply to range
* The only validation rule that's in there is for the *Exercise* field. It currently validates entries for column D, rows 14 through 88
* This won't need to be modified unless you were to want to add new groups or new rows within a group 
##### Criteria
```Drop-down (from a range)```
##### Source of data
This populates the drop down with data from the Exercise Database tab, specifically column D from row 3. There's no limit to 
```='Exercise Database'!$D$3:$D```
