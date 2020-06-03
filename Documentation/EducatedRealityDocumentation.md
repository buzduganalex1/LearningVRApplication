# Educated Reality Documentation

## Description

This is a documentation for describing the functionality added in the educated reality project 

## Building

To build the application the following steps have to be performed:

- File -> Build Settings -> Select the platform (Windows/Android)

### For Oculus Rift S(Windows)

- Click player settings
![alt text](Pictures/WindowsScreen.JPG)

- Make sure the Company name + product name + version are correct and support (they should be in the ```Company.Name.Version```)
![alt text](Pictures/PlayerSettings.JPG)

- Make sure the Configuration/Scripting Backend is set to mono
![alt text](Pictures/Mono.JPG)

- Go back and click build 
![alt text](Pictures/Build.JPG)

- The result should contain an exe, when you want to deliver the application you should zip the whole folder and unzip it on the client.
![alt text](Pictures/Result.JPG)

### For Oculus Quest/Go(Android)

- Click player settings
![alt text](Pictures/WindowsScreen.JPG)

- Click Android and click SwithPlatform (It will take some time to load and prepare everything)
![alt text](Pictures/Loaded.JPG)
![alt text](Pictures/Android.JPG)

- Click Build 
![alt text](Pictures/AndroidBuild.JPG)

- The result should be an __apk__ you can install on quest/go.
![alt text](Pictures/ResultAndroid.JPG)

## Scripts

To use the network functionality Photon folder was added.

All the custom scripts are added in the Assets/Scripts folder
- TalkingPoint
- NextButton
- RetryButton
- SharedObject
- NetworkPlayer

![alt text](Pictures/Scripts.JPG)

### TalkingPoint

This script is used for identifying a TalkingPoint and should be present on each plane you want to display in the machine. Each Talking point has to contain a PhotonTransformView and PhotonView component and has to have the meshRenderer component turned of instead of the first one like in the following picture:

![alt text](Pictures/TalkingPoint.JPG)


### NextButton

This script is used to go through all the talking points. It has a list of talking points as input. When the button is pressed one item from the list will have the MeshRenderer component enabled while all the others disabled.

![alt text](Pictures/NextButton.JPG)


### RetryButton

This script is used to reset the initial position of all the objects from the Card list. It takes as input a list of objects, saves their initial position. When pressed all the items from list will be moved to the initial position.

![alt text](Pictures/Retry.JPG)

### SharedObject

This script is for transfering the position of an object through the network. It should be present on all the object you want to share for all the members of the network. 
The object should look like this.

![alt text](Pictures/SharedObject.JPG)


### NetworkPlayer

The network player is a prefab locate at:
__Assets\Photon\PhotonVoice\Demos\DemoVoiceMinimal\Resources__

This prefab is created when the player connects. Once the player connects this prefab is created and the scripts tells the object to link itself to the VRRig component (to the head). In case you want to change the avatar just modify the prefab to use a head instead of the red cube.

![alt text](Pictures/NetworkPlayer.JPG)

### Voice Connection and Recorder

This component is located in the scene and is responasible for connecting to the network and instatiating the network player.

![alt text](Pictures/Connect.JPG)


# 6/03/2020

## Additional Scripts

More scripts were added :
- AnimalSelection (Script used for getting a random animal name from a list)
- FixedTeleportation (Script that overrides the teleportation area so no chage has to be made when starting the project again)
- InteractionButton (Script that has to be on each button that we want to interact with)
- MovieController (Script that is used for controlling the video player -> pause/stop/next)
- MoviesHolder (Script that holds all the movies somebody can go through)
- ReticleScript (Script attached to the reticle that makes sure the size is correct)

![alt text](Pictures/NewScripts.JPG)

## Animal Machine

![alt text](Pictures/AnimalMachine.JPG)

The animal machine contains 2 canvases
1. holding the UI text that will display the animal name
2. holding the button the customer can press to change the name

For the ray to interact with the button there has to be a rigid body on the button as well as a collider.

On the button there are 2 scripts:
1. InteractionButton (has to be on each button)
2. AnimalSelectionButton

The animal selection button has a list of words that can be displayed on the machine. You can add as many animal names as you like.
In order for the script to work the Text field on the Animal Selection Button has to contain the text ui element from the AnimalNameCanvas (That is the text used to display the animal name).

This machines is synced through the network by using punRPC procedures which are like a signal that is sent to all the clients. When the button is pressed a signal is sent to every other animal machine for all the clients with the current word index and everybody updates their machine.

## Movies Holder
![alt text](Pictures/MoviesHolder.JPG)

On the video player component a script called MoviesHolder was added so you can add a list of movies somebody can go through.
In our example we have videos in Asset folder and we dragged and droped them on the Movies field. 
The VideoPlayer field has to contain the video player component from the object and the current movie index is just informative(do not change that it should always be 0).

In order to interact with the video player there are 3 canvases each holding a button added 
1. NextMovie
2. StopStartMovie
3. BackMovie

![alt text](Pictures/MovieController.JPG)

Each button has a Movie Controller script attached that holds logic for interacting with the video player. Each button has it's own operation that executes when you click on it and you can see it on the OnClick event list. 
NOTE: For the StartStop button we added A stop and a start image so when you click on it the image changes accordingly, those are not neccesary for other buttons.

The buttons work the same as other interaction buttons, having a rigid body, a collider and the interaction button script. 

__In order for the reticle to change size when you hover over a button, the name of the button has to contain "Button" in it.__

The network part is the same as the animal button where every time you click on a movie button it sends a signal to all the other clients connected with the current movie index. Each client updates accordingly on his side the movie that is playing.

## Connection scene

- A new scene was added in order for a better experience in case of a disconnect. The scene contains a simple ui canvas with multiple ui elements and a vr rig that can interact with them. This will later be transformed intro a lobby.

![alt text](Pictures/NewScene.JPG)
Those produce no effect on the application unless for android build. When you build the android version make sure to remove the LevelSelection (isLoading) lines before, (you can remove them is they are (not loaded), if they are is loading, close unity and open again, or just open the LevelSelection scene)

- When somebody presses the connect button the next scene is already loaded asyncronously but this produces some issues in the editor mode apparently

![alt text](Pictures/Issues.JPG)


- Besides this changes to the way a player connects to the network were made. Now instead of joining a random room all the players join to the same room. There are max 10 players that can join right now. In case of disconnect the client will be prompted to the first screen and will have to connect again, if he does not have internet access then he will always come to the first screen.
The LevelSelection Scene has to be the first one and the main room the second one.

![alt text](Pictures/ExtraScene.JPG)


## Fixed some issues 

- fixed the issue with the teleportation angle in the beggining of the project
- If you want the raycast to ignore the targets like the machine it has to have the IgnoreRaycastLayer
- Changed the photon settings of the project to work on asia
- Added a pole to hold the change color button (other than this nothing changed regarding the color button)

![alt text](Pictures/ColorButtonUpdate.JPG)