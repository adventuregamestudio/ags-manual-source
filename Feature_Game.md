## Game

In Adventure Game Studio the game consists of multiple pieces. Not all of them are required for the game to run, most are optional, and should be used only when you need them. But it's a good thing to learn about them all, so to have an idea of what you can do.

When you open the AGS Editor, you will see a "Project Explorer" window, it's a tree-like menu that lets you access every aspect of the game.

Game may be roughly divided on: game settings, game elements, resources and scripts.

Game settings let you configure the overall game's behavior.

Game elements represent various objects and functionality in your game. They act, and may be interacted with when the game runs. These are: Characters, GUI, Dialogs, Inventory Items, Rooms, and few others. Most elements are created right in the Editor, but there are few that may only be created in script, these are sometimes referred as "dynamic objects".

Resources (also called "assets") help game and game's elements to be seen and heard: Sprites, Audio clips and video files, animation Views, Fonts, Translations, and similar things. Game resources may also include extra files, in case you need to package custom data with your game, this option is meant for advanced users.

Finally, scripts are a kind of "programs" within your game. They let you tell the engine what should happen while the game is run.
What you setup in the Editor is only the *starting state* of your game. It's the scripts that make your game alive, actually make something *happen* in it.
Scripts are written in a scripting language, called simply "AGS Script". This manual has a full section dedicated to this language, and available commands.

In AGS the game's flow is based on Rooms. Room is a scene where any game activity takes place. The game may consist of one or many rooms, but only one Room may be loaded and active at the current time. All inactive rooms are unloaded, and only keep their saved states from the last time they were visited. When game switches to another room, the previous one saves its state in game's memory and gets unloaded. If a room was visited once before, then, upon loading it again, its state is restored. That's the default behavior, but you may also override it and make it so that the room does not keep its state, and resets each time it is visited again.

Characters are another essential element of the game. In AGS one of the characters must be assigned as a "playable character" (or "player character"). The idea is that the "player character" is the one which player controls by default, but, more importantly, it's this character that allows to change rooms: engine always loads the room in which the player character is located. Rooms are changed by moving the player character elsewhere.

Note that the player character does not have to be seen on screen at all. It might as well be made invisible, either temporarily or for the whole game. Only its *presence* is necessary for the game to know which room to load.

While the starting "player character" is set in the Editor, you may select virtually *any other* character as playable during gameplay using a script command. This allows, for example, to have multiple playable characters in game, switching between them as the story progresses, or letting player to switch at will.

NOTE: AGS game requires at least 1 Room and 1 Character (set as "player character") to run. Everything else is optional.

See Also: [AGS Tutorial](StartingOff)

### Game genre and gameplay

AGS engine was originally designed focused on the classic "point-and-click" adventure game genre. For that reason many of its elements are primarily purposed to serve their roles in such adventure game.

At the same time, AGS itself does not define your game's genre, and does not restrict the game to the single genre or gameplay style. It only provides you with tools, and lets you use them as you see fit. Rooms are also not restricted to the certain role or look. It's all about what do you put into the game and how do you script it. It is fairly possible to have multiple different genres featured in the same game in different rooms, and it is also possible to have a single room which dynamically changes its looks and behavior as the game progresses.

Game elements have their standard uses, what they were meant for, but also may be used in custom, alternate ways when necessary. Same thing may often be achieved in various ways, ones more efficient than others. It's essential to learn what types of objects are there, what they can do, which commands does the scripting language provide. The more you know about them, the easier it will be to find a good way to create your game.

### Game start and ending

AGS also does not strictly define such things as "game start" or "game end". This may be confusing at first, but the idea is that you define what happens in the game yourself.

For example, you may make it so that the first Room loaded in game acts as a "Main Menu". Within the "Main Menu" there will be a "New Game" button, and once the players press that, they are sent to another Room where the "story" begins.

On another hand, you may make it so that the game story starts right away, without any "main menus", and there's just a floating game menu displayed by clicking on a button in a corner of a screen.

In a similar vein, the game ending is purely scripted by you, and so is what happens when the player reaches the end. You might want to send them back to the "Main Menu", or to the ending credits scene, or even simply shutdown the game. It's all possible to do, and is up to you to decide.

### Game scripts

As been said, what you set in the Editor is how your game will look like when it starts, or how the Rooms will look like when they first load. But Scripts let you to control your game after it has started.

AGS scripts are written in its own language called simply "AGS Script", which syntax is somewhat similar to other existing programming languages, such as C or C#. AGS Script supports various operations and commands provided by the engine, that may change the state of the game or game objects, make game objects move, act, interact with each other, display things on screen, play sounds, and much more.

AGS scripts are event-based. What this means is that the script is not run on its own, but instead certain parts of it (called "functions") are run at predefined events in the game.

For example, just as the game is launched, engine will run "game_start" function, if one is found in your script. When player clicks mouse button engine runs "on_mouse_click" function, when player presses a keyboard key - "on_key_press" function. For things that require continuous updates there is a group of predefined functions named "repeatedly_execute" (and similar) that are run periodically, on each game's tick. When a new room is loaded, engine triggers "Room Load" event, and if there's a function in script connected to that room's event, then such function will be run. There are events for clicking on a Character or a room object, events for pressing buttons on GUI, and so forth.

What happens in these functions is entirely up to you.
Event functions are optional though, you don't have to create functions for all existing events, only for those that you need to.

It's important to know that there are 2 kinds of events in AGS: ones that expect a script function of predefined name to run, and ones that require you to explicitly assign your custom function name to them. The global events that refer to the game itself are of the first kind, while Room and object-related are of the second. Assigning a function to event is done in the Editor, on the "events" tab of a selected game object.

See Also: [Scripting Tutorial](ScriptingTutorial), [Scripting Language](ScriptingLanguage), [Script API](Scripting), [Event Types](EventTypes), [How to assign functions to object events](acintro3)

### Game Templates

When you create a new game in AGS Editor, you choose a Game Template to start with. Please be aware that the Template is only a starting set, but in no way a restriction to what you can do in your game. Regardless of which template do you choose, you can modify anything later: add, change, remove, and even redo all the things that template has created for you from scratch, if you find them unnecessary or conflicting with your game idea. Latter, however, would defeat the purpose of a template, therefore it's useful to learn what each template provides and start with the one best suited for your purpose.

See Also: [Templates](Templates)
