## Global Arrays

There's a number of global arrays declared for you to access certain game objects. For historical reasons some of them have their length defined as a constant, others have their length provided as a property.

Array | Length | Description
--- | --- | ---
Character character[] | [`Game.CharacterCount`](Game#gamecharactercount) | All game's Characters
Dialog dialog[] | [`Game.DialogCount`](Game#gamedialogcount) | All game's dialogs
GUI gui[] | [`Game.GUICount`](Game#gameguicount) | All game's GUI
InventoryItem inventory[] | [`Game.InventoryItemCount`](Game#gameinventoryitemcount) | All game's InventoryItems
Hotspot hotspot[] | AGS_MAX_HOTSPOTS | Current room's Hotspots
Object object[] | [`Room.ObjectCount`](Room#roomobjectcount) | Current room's Room Objects
Region region[] | AGS_MAX_REGIONS | Current room's Regions
ColorType palette[] | PALETTE_SIZE | Game's palette, for 256-color mode

Arrays that contain game objects contain them in the order of their respective ID. For example, if you have a character with the script name "cChar", then `cChar.ID` would tell you the index at which this character is in the `character` array. This may be useful if you have to store its numeric ID in an integer variable for later use and then find the Character by this number.

Abstract example:

```ags
int saved_id;
...
saved_id = cChar.ID;
...
Character* found_char = character[saved_id];
```

Naturally, when you know the array's size it's possible to iterate over it, for example when you want to perform an action on all of these objects, or when you want to find one of the objects in the game array by certain rules:

```ags
for (int i = 0; i < Game.CharacterCount; i++) {
    Character* c = character[i];
    Display("Character number %d got name %s", i, c.DisplayName);
}
```

**IMPORTANT:** there is a known issue that if the game script mentions one of those arrays anywhere, then your game must have at least one object of that type. This is because AGS script cannot have zero-sized arrays and does not declare those that won't have any elements.<br>
This is essential to remember if you're using a game template or script module that works with `dialog[]`, `gui[]` or `inventory[]` arrays: to make your game compile in this case you have to create at least one dummy Dialog, GUI or Inventory item (even if you don't use it).
