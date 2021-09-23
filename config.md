```
ddstats-rust config
    Optional configurations:
        - ui_conf.logo: (String)

|| Game Data Modules:
	DdclOutOfDateWarning
	RunData
    Timer
    Gems
    Homing(SizeStyle) || Minimal, Compact, Full
    Kills
    Accuracy
    GemsLost(SizeStyle) || Minimal, Compact, Full
    CollectionAccuracy
    HomingSplits(Vec<(String, f32)>) || Vec<(String, f32)>: split times and names
    HomingUsed
    DaggersEaten
    Spacing

|| Style Colors
    Reset
    Black
    Red
    Green
    Yellow
    Blue
    Magenta
    Cyan
    Gray
    DarkGray
    LightRed
    LightGreen
    LightYellow
    LightBlue
    LightMagenta
    LightCyan
    White
    Rgb(u8, u8, u8) || Will approximate color if terminal doesn't allow
    Indexed(u8)

    Examples:
        - Rgb(0xF7, 0xCA, 0x88) || this is the hex color #F7CA88 
		
    On windows you can change the named colors (Red, Blue, etc.) by right clicking the
    top of the window and changing CMD preferences.

    On linux it just pulls from your terminal colors, probably in .Xdefaults

Breaking Changes:
    -> ui_conf.column_distance is now necessary (2021-09-19)

```
