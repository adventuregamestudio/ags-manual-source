## Game events order

The game in AGS consists of "loops" which run repeatedly with certain speed (see [SetGameSpeed](Globalfunctions_General#setgamespeed)). On each loop it runs certain events and script callbacks, updates the game and redraws the game on screen.

The order of these actions within a single loop is consistent. When you start using AGS you probably won't care about that, but as you write more complex scripts, it may become important. This order may seem a bit strange in some parts, but it's a result of AGS development history.

Following is a sequence of actions which happen in a normal game loop:
1. [`repeatedly_execute_always`](Globalfunctions_Event#repeatedly_execute_always) function is called (if found in script).
2. (since AGS 3.6.0) Game cursor position update; GUI focus is updated, depending on a new cursor position.
3. Reaction to player's input (keyboard, mouse, and so on); not every action is immediate though, some things will be scheduled and put into the event queue, which is processed much later.
4. *Main game update:* almost everything in the game is updated: characters, room objects, animations, and so forth.
5. [`late_repeatedly_execute_always`](Globalfunctions_Event#late_repeatedly_execute_always) function is called (if found in script).
6. Non-blocking screen shaking update (see [ShakeScreenBackground](Globalfunctions_Screen#shakescreenbackground)).
7. (prior to AGS 3.6.0) Game cursor position update; GUI focus is updated, depending on a new cursor position.
8. *Game is drawn on screen.*
9. All the scheduled events are executed. Strictly speaking, the order of execution is undefined. Commonly [`repeatedly_execute`](Globalfunctions_Event#repeatedly_execute) goes first, but that's rather a coincidence and is not recommended to rely upon. Other events include: `on_key_press`, `on_mouse_click`, `on_event`, and so forth (see [their respective page](Globalfunctions_Event#global-event-handlers) for details).

When a event is executed, the engine searches game scripts for the corresponding function, and runs that function in each script where it's found. This is done in the following order:
 - room script first,
 - script modules, starting from the topmost in the project's list of scripts, and going down to the last one.

This sequence is repeated separately for each event, so first all functions in all scripts related to the first scheduled event are run, then all functions related to the second event are run, and so forth.
