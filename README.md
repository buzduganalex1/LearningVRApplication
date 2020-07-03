# Educated Reality Documentation

## Description

This is a documentation for describing the functionality added in the educated reality project 

## Building

To build the application the following steps have to be performed:

- File -> Build Settings -> Select the platform (Windows/Android)

### For Oculus Rift S(Windows)

- Click player settings
![alt text](Pictures/WindowsScreen.jpg)

- Make sure the Company name + product name + version are correct and support (they should be in the ```Company.Name.Version```)
![alt text](Pictures/PlayerSettings.jpg)

- Make sure the Configuration/Scripting Backend is set to mono
![alt text](Pictures/Mono.jpg)

- Go back and click build 
![alt text](Pictures/Build.jpg)

- The result should contain an exe, when you want to deliver the application you should zip the whole folder and unzip it on the client.
![alt text](Pictures/Result.jpg)

### For Oculus Quest/Go(Android)

- Click player settings
![alt text](Pictures/WindowsScreen.jpg)

- Click Android and click SwithPlatform (It will take some time to load and prepare everything)
![alt text](Pictures/Loaded.jpg)
![alt text](Pictures/Android.jpg)

- Click Build 
![alt text](Pictures/AndroidBuild.jpg)

- The result should be an __apk__ you can install on quest/go.
![alt text](Pictures/ResultAndroid.jpg)

## Scripts

To use the network functionality Photon folder was added.

All the custom scripts are added in the Assets/Scripts folder
- TalkingPoint
- NextButton
- RetryButton
- SharedObject
- NetworkPlayer

![alt text](Pictures/Scripts.jpg)

### TalkingPoint

This script is used for identifying a TalkingPoint and should be present on each plane you want to display in the machine. Each Talking point has to contain a PhotonTransformView and PhotonView component and has to have the meshRenderer component turned of instead of the first one like in the following picture:

![alt text](Pictures/TalkingPoint.jpg)


### NextButton

This script is used to go through all the talking points. It has a list of talking points as input. When the button is pressed one item from the list will have the MeshRenderer component enabled while all the others disabled.

![alt text](Pictures/NextButton.jpg)


### RetryButton

This script is used to reset the initial position of all the objects from the Card list. It takes as input a list of objects, saves their initial position. When pressed all the items from list will be moved to the initial position.

![alt text](Pictures/Retry.jpg)

### SharedObject

This script is for transfering the position of an object through the network. It should be present on all the object you want to share for all the members of the network. 
The object should look like this.

![alt text](Pictures/SharedObject.jpg)


### NetworkPlayer

The network player is a prefab locate at:
__Assets\Photon\PhotonVoice\Demos\DemoVoiceMinimal\Resources__

This prefab is created when the player connects. Once the player connects this prefab is created and the scripts tells the object to link itself to the VRRig component (to the head). In case you want to change the avatar just modify the prefab to use a head instead of the red cube.

![alt text](Pictures/NetworkPlayer.jpg)

### Voice Connection and Recorder

This component is located in the scene and is responasible for connecting to the network and instatiating the network player.

![alt text](Pictures/Connect.jpg)


# 6/03/2020

## Additional Scripts

More scripts were added :
- AnimalSelection (Script used for getting a random animal name from a list)
- FixedTeleportation (Script that overrides the teleportation area so no chage has to be made when starting the project again)
- InteractionButton (Script that has to be on each button that we want to interact with)
- MovieController (Script that is used for controlling the video player -> pause/stop/next)
- MoviesHolder (Script that holds all the movies somebody can go through)
- ReticleScript (Script attached to the reticle that makes sure the size is correct)

![alt text](Pictures/NewScripts.jpg)

## Animal Machine

![alt text](Pictures/AnimalMachine.jpg)

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
![alt text](Pictures/MoviesHolder.jpg)

On the video player component a script called MoviesHolder was added so you can add a list of movies somebody can go through.
In our example we have videos in Asset folder and we dragged and droped them on the Movies field. 
The VideoPlayer field has to contain the video player component from the object and the current movie index is just informative(do not change that it should always be 0).

In order to interact with the video player there are 3 canvases each holding a button added 
1. NextMovie
2. StopStartMovie
3. BackMovie

![alt text](Pictures/MovieController.jpg)

Each button has a Movie Controller script attached that holds logic for interacting with the video player. Each button has it's own operation that executes when you click on it and you can see it on the OnClick event list. 
NOTE: For the StartStop button we added A stop and a start image so when you click on it the image changes accordingly, those are not neccesary for other buttons.

The buttons work the same as other interaction buttons, having a rigid body, a collider and the interaction button script. 

__In order for the reticle to change size when you hover over a button, the name of the button has to contain "Button" in it.__

The network part is the same as the animal button where every time you click on a movie button it sends a signal to all the other clients connected with the current movie index. Each client updates accordingly on his side the movie that is playing.

## Connection scene

- A new scene was added in order for a better experience in case of a disconnect. The scene contains a simple ui canvas with multiple ui elements and a vr rig that can interact with them. This will later be transformed intro a lobby.

![alt text](Pictures/NewScene.jpg)
Those produce no effect on the application unless for android build. When you build the android version make sure to remove the LevelSelection (isLoading) lines before, (you can remove them is they are (not loaded), if they are is loading, close unity and open again, or just open the LevelSelection scene)

- When somebody presses the connect button the next scene is already loaded asyncronously but this produces some issues in the editor mode apparently

![alt text](Pictures/Issues.jpg)


- Besides this changes to the way a player connects to the network were made. Now instead of joining a random room all the players join to the same room. There are max 10 players that can join right now. In case of disconnect the client will be prompted to the first screen and will have to connect again, if he does not have internet access then he will always come to the first screen.
The LevelSelection Scene has to be the first one and the main room the second one.

![alt text](Pictures/ExtraScene.jpg)


## Fixed some issues 

- fixed the issue with the teleportation angle in the beggining of the project
- If you want the raycast to ignore the targets like the machine it has to have the IgnoreRaycastLayer
- Changed the photon settings of the project to work on asia
- Added a pole to hold the change color button (other than this nothing changed regarding the color button)

![alt text](Pictures/ColorButtonUpdate.jpg)

# 7/03/2020

Updates

- Introduced 3 new scenes
- Cleaned up the project 
- Structured scripts in folders
- Added head + hair + body
- Added hand syncronisation
- Added room creation + joinin + character customisation
- A second room was added
- Transition between scenes was added

We added 3 new scenes and this is the current order of the scenes. Order is really important and should be respected because based on order we load our scenes. 

![alt text](Pictures/NewScenes.jpg)

## New Scenes and Project Structure

### Quote scene

![alt text](Pictures/QuoteScene.jpg)

Is the first scene of the game, it shows a random quote then waits for 5 seconds then loads LevelSelection. Here there is one very important component that is present in all scenes called LevelLoader. 

![alt text](Pictures/LevelLoader.jpg)

LevelLoader is big black canvas with a image covering the screen. After the scene starts the image has an animation to fade away so the user can see the real world. This component is also responsable for loading the scenes in the game, this is done via the code.

![alt text](Pictures/LevelLoaderCode.jpg)

We use a coroutine that starts the scene loading before actual load so we won't have a stutter in the transition. First the StartLoading is invoked with the scenenNumber we have to know then when we want to perform the actual loading we set ready = true;

__IMPORTANT IS TO ALWAYS HAVE THIS ACTIVE ON EACH SCENE__ (Note: you can deactivate it while working in the scene, but have to make sure that is enabled when you build the application)


The other script in this scene is the quotes loader. This scripts reads quotes from a very big json full of quotes and selects a random one to display 

![alt text](Pictures/QuotesLoader.jpg)

The json file is located at __Asset/Resources/quotes.json__ and has a structure where a quote is comprised of a quoteText and a quoteAuthor. 

![alt text](Pictures/QuotesJson.jpg)

If you'd like to use your own quotes, i suggest to either let me help if you want to make a conversion from a different format or use the quotes-short.json instead and add them there.

For that you have to change something in the code : replace "quotes" with "quotes-short".

![alt text](Pictures/QuotesLoaderCode.jpg)

### LevelSelection

![alt text](Pictures/LevelSelection.jpg)

This is the scene where the player can create/join the room. After the players creates/joins a room the the next scene loaded would be either 
The levelSelection scene has the following components in place:

- DatabaseManager
- MenuManager 
- DisableKeyboard
- Several Canvases (CreateRoomCanvas, MainCanvas, CreateCharacterCanvas, JoinRoomCanvas)
- A BodyLoader component

#### PlayerData

This is very important to know, the PlayerData object is used for storing UI information that was selecting and passing it to the main room so we know what to load, where to connect, what our name was.

![alt text](Pictures/PlayerData.jpg)
![alt text](Pictures/PlayerDataCode.jpg)

Every UI action saves information by calling the SetData method and retrieves information by calling GetData. The whole component is flexible and dinamic so we can add more information on the object that can be set before, or during the player gametime. Since this object will be loaded in the room scenes we have to make sure the PlayerData component remains active.

#### MenuManager
![alt text](Pictures/MenuManager.jpg)

The menu manager is a component responsable of registering the canvases we have in our scene. It makes sure that there is always only 1 canvas loaded in the scene and we can navigate between them. Some buttons use the MenuManager to enable only a precise canvas that will be activated.
![alt text](Pictures/NavigateToMenu.jpg)

The component always knows to store the previous menu loaded so we can always go back
![alt text](Pictures/MenuManagerCode.jpg)

This is a key component in the UI selection because is enable coordonation and loading of desired menus.

#### Menus

There are several canvases in the scene :
##### MainCanvas 

![alt text](Pictures/MainCanvas.jpg)

This is the first canvas and should be the only one enabled when the scene starts. All the other canvases have to be closed. This is a simple one, it has 2 buttons (Create/Join). When create is pressed it loads the create_room_canvas when the join is pressed it loads the join_room canvas.

##### CreateRoomCanvas 
![alt text](Pictures/CreateRoomCanvas.jpg)

This is the canvas used for creating a room. There are 3 fields added and some validation for each field to make sure the data is correct. You can also a component that acts like a carrousel and goes through the room images. To make it work we can add a list of images and the name of the image is the room type name that will be saved in photon  (In theory if we name this images the room index in the build settings we should be able to add easily multiple rooms)

![alt text](Pictures/ImageCanvas.jpg)

After the data is filled and the room selected when create is pressed a new room is created and it will lead you to the create character canvas

##### JoinRoomCanvas

![alt text](Pictures/JoinRoomCanvas.jpg)

This the menu used for joining a room, it has a simple validation to make sure the data si filled and that the room exists when you click join. After join is clicked it will load the create character canvas.

##### CreateCharacterCanvas
![alt text](Pictures/CreateCharacterCanvas.jpg)

The create character canvas is the place where the player can customize his character. It has a name input with a simple validation.

There is one more important component here called the __BodySelectionScript__

![alt text](Pictures/BodyPartsSelector.jpg)

In order to select a body part there are 2 containers that hold all the possible body parts :
- Head Parent
- BodyParent 

The scripts take as input a list of those body parts that can be selected and makes sure to keep only 1 active.

In order to add a new Body/Head you have to add a similar head/body under HeadParent/BodyParent -> disable it -> and then drag and drop it in the body parts list of the desired BodySelectionScripts

#### Database component

Since photon doesn't store information like rooms created after the session is closed we have added a mongoDb database hosted in singapore
![alt text](Pictures/DatabaseManager1.jpg)

We use this database to store information in the form of EducatedRealityRoom object, more data can be added to the object.



### Main room or Main room2. 

Some changes were performed on the room scenes and here is the structure of the room scenes now:
![alt text](Pictures/Room1.jpg)

I grouped the machines under one object to clean it up a bit, and also the video players. For the video players all of them can use the same script the same as i did in the rooms.

3 new objects were added in the scene and have to be present in all the rooms the player will join:
- RoomConnector
- BodyPartsLoader
- NetworkDisconnector
- (This is used only for testing so it will work when you press play in editor) TestPlayerData __NOTE__ THIS HAS TO BE DISABLED WHEN THE BUILD IS MADE.

#### RoomConnector

![alt text](Pictures/RoomConnector.jpg)

This script is used to read the room name + room password and provide a room name we can use to create a room in photon. Because of photon limitation the rooms we create are all in the following format __roomname-roompassword__ (Example: roomName = test | roomPassowrd = link => photon room name = test-link).

This scripted is used both by the voice connection and by the player connection to join the correct room.

![alt text](Pictures/RoomConnectorCode.jpg)


#### BodyPartsLoader

![alt text](Pictures/BodyLoader.jpg)

This component is responsable to create the head and the body for each player and send them to the network once the player connected. You will notice that to make sure the head and the head will fit and will look nice we created some containers for the body and for the head. The script needs to know those containers so it instatiates them in the correct place. To experiment with new head or body you can just drag them in the correct place and see if they fit, then disable them when you want to build. 

This script also loads the player name above it's head. The name is loaded under the body container because we want it stable as we move and not move with the head. The object instatiated is also present in the scene to test and is called Name. You can see it under BodyContainer. The name is a canvas that has a text that is being populated when the player connects.

![alt text](Pictures/BodyPartsLoaderCode.jpg)

I will make a specific section for this but to bodies and heads are loaded from :
![alt text](Pictures/BodyPartsLocationPhoton.jpg)

If you want a new body or a head you have to add a similar object in that location with name Body/Head and a new number.

#### NetworkDisconnector

![alt text](Pictures/NetworkDisconnector.jpg)

Another important component is the network disconnector. Since we have multiple scenes loading asyncronously and also we can go back and forward between scenes there are scripts that are not destroyed between scenes like the VoiceConnectionAndRecorder. This script is responsable for connecting us to the network. The networkDisconnector is used to destroy this object and to load the first scene of the game after that. This is the same component that is used when the back button is pressed in the menu.

__When creating a new room make sure to have all those set up__ (If you copy/paste the rooms then it should be ok)
