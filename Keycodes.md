## Key code table

In AGS script you have a `eKeyCode` enumeration that defines key codes. These key codes can be passed to
[`on_key_press`](Globalfunctions_Event#on_key_press), as well as used in calls to [`IsKeyPressed`](Globalfunctions_General#iskeypressed).

Historically AGS has Ctrl+X key combinations have their own distinct keycodes. At the same time, keycodes corresponding to the modifier keys (Shift, Ctrl and Alt) were never passed to the [`on_key_press`](Globalfunctions_Event#on_key_press) alone and were only allowed with [`IsKeyPressed`](Globalfunctions_General#iskeypressed).

Starting from version **3.6.0** AGS supports two ["key handling modes"](WhatsNewIn36#changes-to-key-input-handling). The old mode works as described above, according to the historical behavior. The new mode actually allows modifier keycodes in `on_key_press`, and disables combined keycodes like Ctrl+X: instead these are passed as two separate keycodes.

Following section lists the members of this enum, and which keys they represent.

AGS keycode | Key | Numeric value
--- | --- | ---
`eKeyNone` | none | 0
`eKeyCtrlA` | Ctrl+A | 1
`eKeyCtrlB` | Ctrl+B | 2
`eKeyCtrlC` | Ctrl+C | 3
`eKeyCtrlD` | Ctrl+D | 4
`eKeyCtrlE` | Ctrl+E | 5
`eKeyCtrlF` | Ctrl+F | 6
`eKeyCtrlG` | Ctrl+G | 7
`eKeyCtrlH` | Ctrl+H | 8
`eKeyCtrlI` | Ctrl+I | 9
`eKeyCtrlJ` | Ctrl+J | 10
`eKeyCtrlK` | Ctrl+K | 11
`eKeyCtrlL` | Ctrl+L | 12
`eKeyCtrlM` | Ctrl+M | 13
`eKeyCtrlN` | Ctrl+N | 14
`eKeyCtrlO` | Ctrl+O | 15
`eKeyCtrlP` | Ctrl+P | 16
`eKeyCtrlQ` | Ctrl+Q | 17
`eKeyCtrlR` | Ctrl+R | 18
`eKeyCtrlS` | Ctrl+S | 19
`eKeyCtrlT` | Ctrl+T | 20
`eKeyCtrlU` | Ctrl+U | 21
`eKeyCtrlV` | Ctrl+V | 22
`eKeyCtrlW` | Ctrl+W | 23
`eKeyCtrlX` | Ctrl+X | 24
`eKeyCtrlY` | Ctrl+Y | 25
`eKeyCtrlZ` | Ctrl+Z | 26
`eKey0` | 0 | 48
`eKey1` | 1 | 49
`eKey2` | 2 | 50
`eKey3` | 3 | 51
`eKey4` | 4 | 52
`eKey5` | 5 | 53
`eKey6` | 6 | 54
`eKey7` | 7 | 55
`eKey8` | 8 | 56
`eKey9` | 9 | 57
`eKeyA` | A | 65
`eKeyB` | B | 66
`eKeyC` | C | 67
`eKeyD` | D | 68
`eKeyE` | E | 69
`eKeyF` | F | 70
`eKeyG` | G | 71
`eKeyH` | H | 72
`eKeyI` | I | 73
`eKeyJ` | J | 74
`eKeyK` | K | 75
`eKeyL` | L | 76
`eKeyM` | M | 77
`eKeyN` | N | 78
`eKeyO` | O | 79
`eKeyP` | P | 80
`eKeyQ` | Q | 81
`eKeyR` | R | 82
`eKeyS` | S | 83
`eKeyT` | T | 84
`eKeyU` | U | 85
`eKeyV` | V | 86
`eKeyW` | W | 87
`eKeyX` | X | 88
`eKeyY` | Y | 89
`eKeyZ` | Z | 90
`eKeyAmpersand` | & | 38
`eKeyAsterisk` | * | 42
`eKeyAt` | @ | 64
`eKeyBackSlash` | \ | 92
`eKeyBackspace` | Backspace | 8
`eKeyCloseBracket` | ] | 93
`eKeyCloseParenthesis` | ) | 41
`eKeyColon` | : | 58
`eKeyComma` | , | 44
`eKeyDelete` | Delete | 383
`eKeyDollar` | $ | 36
`eKeyDoubleQuote` | " | 34
`eKeyEquals` | = | 61
`eKeyEscape` | Escape | 27
`eKeyExclamationMark` | ! | 33
`eKeyForwardSlash` | / | 47
`eKeyGreaterThan` | > | 62
`eKeyHash` | # | 35
`eKeyHyphen` | - | 45
`eKeyInsert` | Insert | 382
`eKeyLessThan` | < | 60
`eKeyOpenBracket` | [ | 91
`eKeyOpenParenthesis` | ( | 40
`eKeyPercent` | % | 37
`eKeyPeriod` | . | 46
`eKeyPlus` | + | 43
`eKeyQuestionMark` | ? | 63
`eKeyReturn` | Return | 13
`eKeySemiColon` | ; | 59
`eKeySingleQuote` | ' | 39
`eKeySpace` | Space | 32
`eKeyTab` | Tab | 9
`eKeyUnderscore` | _ | 95
`eKeyF1` | F1 | 359
`eKeyF2` | F2 | 360
`eKeyF3` | F3 | 361
`eKeyF4` | F4 | 362
`eKeyF5` | F5 | 363
`eKeyF6` | F6 | 364
`eKeyF7` | F7 | 365
`eKeyF8` | F8 | 366
`eKeyF9` | F9 | 367
`eKeyF10` | F10 | 368
`eKeyF11` | F11 | 433
`eKeyF12` | F12 | 434
`eKeyHome` | Home | 371
`eKeyUpArrow` | Up arrow | 372
`eKeyPageUp` | Page up | 373
`eKeyLeftArrow` | Left arrow | 375
`eKeyNumPad5` | Numeric keypad 5 | 376
`eKeyRightArrow` | Right arrow | 377
`eKeyEnd` | End | 379
`eKeyDownArrow` | Down arrow | 380
`eKeyPageDown` | Page down | 381
`eKeyShiftLeft` | Left Shift | 403
`eKeyShiftRight` | Right Shift | 404
`eKeyCtrlLeft` | Left Ctrl | 405
`eKeyCtrlRight` | Right Ctrl | 406
`eKeyAltLeft` | Left Alt | 407
`eKeyAltRight` | Right Alt | 420

Use these keycodes in your
[`on_key_press`](Globalfunctions_Event#on_key_press) function to
process player input. For example:

```ags
if (keycode == eKeyA) Display("You pressed A");
if (keycode == eKeyPlus) Display("You pressed the Plus key");
```

*Compatibility:* the mod key constants (eKeyShiftLeft to eKeyAltRight) are only supported since AGS 3.6.0. In previous versions you'd have to use corresponding numeric values.

### Key modifiers

In addition, there's separate `eKeyMod` enumeration with modifier "flags". These values are special so that they may be combined in one variable to make a list of modifiers (like, Shift + Ctrl). This enumeration is supported since **AGS 3.6.0**. Following is the list of possible values:

AGS modifier | Mod |
--- | --- 
`eKeyModShiftLeft` | Left Shift 
`eKeyModShiftRight` | Right Shift 
`eKeyModShift` | any Shift 
`eKeyModCtrlLeft` | Left Ctrl 
`eKeyModCtrlRight` | Right Ctrl 
`eKeyModCtrl` | any Ctrl 
`eKeyModAltLeft` | Left Alt 
`eKeyModAltRight` | Right Alt 
`eKeyModAlt` | any Alt 
`eKeyModNum` | Num Lock 
`eKeyModCaps` | Caps Lock 

You may use these mod codes in the extended variant of [`on_key_press`](Globalfunctions_Event#on_key_press) function to check which mod keys were pressed along with the primary key. Remember that since these mods are "flags", they should be tested using not the regular "comparison" operator `==`, but bitwise operators. For example:

```ags
if (keycode == eKeyA) {
    if (mod & eKeyModCtrl) {
        Display("Pressed Ctrl+A");
    }
}

if (mod & eKeyModAlt == 0) {
    Display("NO Alt was pressed this time");
}
```


In addition, there are two constants defining bitwise masks, one for key codes, and another for key modifiers:

Constant | Meaning 
-- | --
eKeyCodeMask | all key codes
eKeyModMask | all key mods

If both key code and mods are combined in one integer variable, using bitwise operations, these masks may then be used to extract code and mods:

```ags
int combokey = eKeyA | eKeyModCtrlLeft;

int keycode = combokey & eKeyCodeMask; // keycode now contains only eKeyA
int keymod = combokey & eKeyModMask; // keymod now contains only eKeyModCtrlLeft
```

*Compatibility:* eKeyMod enumeration is only supported since AGS 3.6.0.
