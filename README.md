# Unity XR Project Walkthrough Setup Guide Tutorial
Generic Unity 3d XR virtual reality project using XRI toolkit, OpenXR, Oculus SDK and input system.

## Overview ##

Step by step instructions on how to build the base 3d repository / project for simple 3d XR unity projects.. lazy programmers -> clone one of the releases to access this.

when building Vr projects need to change the xr plugin -> android settings for the specific device we are building for. If not OpenXR builds will display in your oculus home room, there we will need independent builds for oculus and friends. This guide will instruct you. and is based off from https://www.youtube.com/watch?v=yxMzAw2Sg5w thank you.

This is easy human, very easy. You will be a meta pro in no time using simple guide.

## BUILDING 3D VR PROJECT WITH MOST RECENT UNITY SETTINGS : PART 1 - SETUP ##

### Step 1 ###

First create a new project in unity 2021.2.10f1 using the 3d template

Once that is done, open up the project settings in the edit menu. Then in the pc tab configure the xr plugin to use Open XR with oculus selected. Then on the android tab in 
the same settings windowselect oculus not OpenXR. *NOTE* Oculus quest drivers are currently not stable within unity to oculus link, so select openXR and add oculus to the 
list of support devices from the drop down.. (or index or vive if you have that) This is the configure used to stream your app onto the device for devolopment. 

This will automagically install the required packages and drivers to your project.

*IMPORTANT* you can change the rendering mode setting on this page and tabs to `Multipass` from single pass for addition depth.. However this will render your scene twice which
requires alot more CPU budget reducing your frame timing to about 5.1ms per lense at 90 hertz. Either settings will work. Also toggle off oculus (1) in the android tab if you do not 
plan on developing for quest 2.

![Figure 01](Documents/Images/01.png)

![Figure 02](Documents/Images/02.png)

### Step 2 ###

Now we need to turn on preview packages in our package manager. open up the package manager and click the gear setting icon in the top right. toggle on preview packages. *NOTE* the
current package manager is broken, and will not display the preview packages by default so we need to manually add the XRI toolkit by click the add drop down icon in the top left of 
the package manager. Select `from git repo` from the drop down, input `com.unity.xr.interative.toolkit` and click add. Click `i understand` when prompted..

![Figure 03](Documents/Images/03.png)

![Figure 04](Documents/Images/04.png)

![Figure 05](Documents/Images/05.png)

### Step 3 ###

Select the new XRI toolkit package in your list and expand the import tree, to display the extra assets to import. Import `defaul input actions`

![Figure 06](Documents/Images/06.png)

will import these new assets, they are just some preset files we will use for our input system

![Figure 07](Documents/Images/07.png)

these are the project runtime and buid settings stored as scriptable objects.. smart

![Figure 08](Documents/Images/08.png)

default xr input system using unity input system (new)

![Figure 09](Documents/Images/09.png)

apply default action presets by clicking on button in inspector on each of the items in

![Figure 10](Documents/Images/10.png)

do this for each one

![Figure 11](Documents/Images/11.png)

![Figure 12](Documents/Images/12.png)

![Figure 13](Documents/Images/13.png)

![Figure 14](Documents/Images/14.png)

![Figure 15](Documents/Images/15.png)

sets up the unity input system controls for headset, and left and right controls

![Figure 16](Documents/Images/16.png)

important user selection actions. this is very human, easy.

![Figure 17](Documents/Images/17.png)

edit the project settings for the preset manager. tells unity project to use our new XRI settings. should have 5 total

![Figure 18](Documents/Images/18.png)

current version of unity has a bug where the left and right input controllers are not filtered, because we use one input system configuration file. enter right and left in the filter

![Figure 19](Documents/Images/19.png)

### Step 4 ###

create ground plane

![Figure 20](Documents/Images/20.png)

if XR installed okay, you should have these XR opens in the right click create menu

![Figure 21](Documents/Images/21.png)

create XR Origin object in scene this has been renamed from XR Rig to avoid name collision with Animation rig naming

![Figure 22](Documents/Images/22.png)

xr origin is just the head. unity will create a manager and orgin object, the cam rig and L/R controllers are represented as child objects so that they inheirt the transform.position of the headset.

![Figure 23](Documents/Images/23.png)

on XR Interation Manager add a new component 'Input Action Manager'

![Figure 24](Documents/Images/24.png)

![Figure 25](Documents/Images/25.png)

add the default action we imported on to the new input action manager component

![Figure 26](Documents/Images/26.png)

we should now see the camera fullcrum in the scene view after setting these input actions

![Figure 27](Documents/Images/27.png)

Camera Offset game object is managed by the XR controller scripts so it should not be modified. This handles our offset height we are from the floor setting within gaurdian. left hand should have the correct input's that we filtered before

![Figure 28](Documents/Images/28.png)

as should the right controller game object inside of the XR Origin game object. hook up oculus with link, and click play, should work. there is a known bug within OpenXR 1.3x version where actions sets are already defined,, reports error in console but game will play okay.

```
ActionSets already attached
UnityEngine.GUIUtility:ProcessEvent (int,intptr,bool&)
```

*MORE INFO* https://forum.unity.com/threads/openxr-1-3-1-actionsets-already-attached-console-logerror.1226619/

```
With OpenXR the recommendation is to use specific device bindings, because XRController is just a macro that expands to bindings 
within all the selected interaction profiles, it has no runtime benefit with OpenXR. This means it is only a shortcut for entering 
bindings and not a means of providing cross platform support. The idea is to be very specific about which controls you want mapped
to which actions per device to ensure the best gameplay on each device. Remember that the runtime is taking care of converting
controller bindings from one device to another in the event that you did not configure bindings for particular device, not unity, 
so you only need to specify bindings for the devices that you want to specifically configure and let the runtime do the rest. This 
is how OpenXR provides future proofing your games and applications since they would not need to be recompiled for newer hardware
that was not known of at the time the game was created, the runtime just needs to provide a map of controls from the devices that 
were configured to ones it knows about.

The current plan for OpenXR 2.0 will be later in 2022 and will require unity 2022+, but its early so any of that could still change. 
In 2.0 we are moving even more towards specific device bindings and away from XRController.
```

**so in sort this will be resolved** So to work around is to only select your dev device that you need to use, again these should be done in separate branches on large projects.. do not use the oculus driver for running local use the OpenXR, its more supported

## BUILDING 3D VR PROJECT WITH MOST RECENT UNITY SETTINGS : PART 2 - LOCOMOTION  ##

### Step 1 ###

add locomotion action based game object to the scene

![Figure 29](Documents/Images/29.png)

![Figure 30](Documents/Images/30.png)

drag the locomotion system game object onto the XR origin editor field in the locomotion system script

![Figure 31](Documents/Images/31.png)

![Figure 32](Documents/Images/32.png)

add teleportation area script onto the ground object

![Figure 33](Documents/Images/33.png)

![Figure 34](Documents/Images/34.png)

*NOTE:* teleportation anchor script is used to teleport onto a specific area on a teleportation area, such as a seat or chair

change ground material to a color other then white so that we can see our hand controller's  laser pointers. Lasers will turn white onto areas that you can teleport onto

create material dir in root of project asset folder

![Figure 35](Documents/Images/35.png)

![Figure 36](Documents/Images/36.png)

### Step 2 ###

create new material in that folder

![Figure 37](Documents/Images/37.png)

name it M_Ground where M_ is prefixed onto all of our materials see style guide for more information

![Figure 38](Documents/Images/38.png)

click on the material and in the inspector set the color to dark drag .. the setting is albedo

![Figure 39](Documents/Images/39.png)

click and drag new material from project window onto the ground plane in your scene view

![Figure 40](Documents/Images/40.png)

now play test with oculus link in your vr headset. you should be able to use left or right grip button to teleport around the teleportation area that we defined as the ground plane. This is very human, easy.

### Step 3 ###

disable teleportation provider and snap turn provider on the locomotion system. we need to do this before we implement continous movement and turn. NOTE  this can cause motion sickness on people new to vr so this should be a toggle option in your game settings so that they can teleport until they get used to vr movement and dont fall down and hurt themselves. important things

![Figure 41](Documents/Images/41.png)

add continuous move provider action based script on the locomotion system 

![Figure 42](Documents/Images/42.png)

add continous turn provider script on the locomotion system

![Figure 43](Documents/Images/43.png)

![Figure 44](Documents/Images/44.png)

![Figure 45](Documents/Images/45.png)

define our loco system on these two new component scripts

![Figure 46](Documents/Images/46.png)

![Figure 47](Documents/Images/47.png)

configure move and turn to use left stick for move and right for turn

![Figure 48](Documents/Images/48.png)

![Figure 49](Documents/Images/49.png)

now play test in your scene on the headset and you should be able to move around and not teleport..

```
IMPROVEMENT: a better control scheme should be that instead of turn continously we should strafe left and right. 
Then we turn based on where we are looking at in the headset. similiar to how you turn on FPS games.. maybe try 
implementing the rotate towards script found in the character controller of the bad cat engine, no games right
analog turns players this is super wonky, also this will allow us to push the right stick forward to sprint while 
the left stick is forward
```

This is very human, easy.

## BUILDING 3D VR PROJECT WITH MOST RECENT UNITY SETTINGS : PART 3 - GRABBING  ##

### Step 1 ###

first create a 3d cube

![Figure 50](Documents/Images/50.png)

make sure to set its origin transform to 0,0,0

![Figure 51](Documents/Images/51.png)

set the scale of the cube to .2 so that its easy to pick up with one hand

![Figure 52](Documents/Images/52.png)

now position the cube infront of our camera , the directon the xr orgin is  facing (blue axis, z+) and set the y to .1 and z to 1

![Figure 53](Documents/Images/53.png)

now we add the xr grab interactable script to the cube

![Figure 54](Documents/Images/54.png)

this will automagically add a rigidbody (physics) and script onto the object

![Figure 55](Documents/Images/55.png)

set the RB (rigidbody) to use continuos collision detection so that the physics are not janky

![Figure 56](Documents/Images/56.png)

in the grab interactable script toggle on smooth position and smooth rotation

![Figure 57](Documents/Images/57.png)

![Figure 58](Documents/Images/58.png)

select velocity tracking option in the movement type drop down menu

![Figure 59](Documents/Images/59.png)

this makes it so that we can not move objects through the wall. kinematic option takes into account the rigidbodys momentum and velocity but can go through walls

this also addes a few more options in the tracking settings

![Figure 60](Documents/Images/60.png)

that allows us to fine tune the dampers on the object tracking,, adjusts the lerp of the transform position. instantanseous will teleport the object.. these cost performance to keep that in mind when using IK on various objects.

now play test.

### Step 2 ###

We need to disable the object anchors on the cube we grab. This setting tells the XRI system to allow the tracking controller analog sticks to be able to move the cube toward or away, and rotate while grabbed, this is useful if we are building stuff and want to be able to specifically place something. select the left and right controller in the scene hierachy

![Figure 61](Documents/Images/61.png)

 scroll the inspector down to show this script that is attached to both controllers

![Figure 62](Documents/Images/62.png)

uncheck anchor control

![Figure 63](Documents/Images/63.png)

now when you play test you can not rotate or move the cube when your grabbing it

This is very human, easy

## *BUILDING 3D VR PROJECT WITH MOST RECENT UNITY SETTINGS : PART 4  - BUILD RUNTIME ##

### Step 1 ###
this section outlines the minimal process required to build your project for distribution. This should be able to generate an APK which you can share

go to build settings
 
![Figure 64](Documents/Images/64.png)

now click and drag the scene object in your hierachy window into the scenes in build box on the build settings to add it to the build

![Figure 65](Documents/Images/65.png)

select the android option on the bottom left of the window

![Figure 66](Documents/Images/66.png)

if the build and run button on the bottom right is grayed out, click the switch platform button to the right of it to switch to the oculus platform sdk. wait a few minutes while all the code switches over and recompiles. now click on the build which will create a sharable executable and also put the apk.

![Figure 67](Documents/Images/67.png)

make a new a new directory on your desktop or anywhere you prefer

![Figure 68](Documents/Images/68.png)

![Figure 69](Documents/Images/69.png)

**SUCCESS, GOOD JOB HUMAN; VERY EASY** you just built your first oculus app

### Step 2 ###

Now lets put it on the device. this used to be a pain, but oculus has a easy to use piece of software that you can use to manage your dev, its call `dev hub`.

https://developer.oculus.com/downloads/package/oculus-developer-hub-win/

![Figure 70](Documents/Images/70.png)

click the download and install and make sure to run as administrator; lastly click install
 
![Figure 71](Documents/Images/71.png)

click finish and make sure to click install
 
![Figure 72](Documents/Images/72.png)
 
opening it should look like

![Figure 73](Documents/Images/73.png)
 
### Step 3 ###

click on the download option in the left menu and then select other packages, look for Oculus ADB drivers

![Figure 74](Documents/Images/74.png)

click download and install, this with automagically configure these. on this setting tab you should see that your ADB is link properly

![Figure 75](Documents/Images/75.png)

also on the other packages page in downloads its a good idea to install the CLI tools by installing the Platform utils CLI

![Figure 76](Documents/Images/76.png)

this will allow us to be able to write scripts to automagically push and distribute our devices which is especially useful when using unities scriptable build tools

you can find all the goodies to this util here https://developer.oculus.com/resources/publish-reference-platform-command-line-utility/

click on your device manager and drag the apk your built onto the right panel

![Figure 77](Documents/Images/77.png)

![Figure 78](Documents/Images/78.png)

click launch after it copies over and viola you have a working app manually installed onto your oculus... easy peasy only 78 steps for human easy.

** GOOD JOB HUMAN :)* you are awarded one smoothie as your treat. 

*batteries not included*

