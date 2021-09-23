# API Docs
ddstats-rust runs a websocket/http server on port 13666 that only accepts requests from localhost.

## Websocket messages

### gimme
returns everything it can from the game's memory including stats frames

### clr-set
sets color for view in color edit mode (F3)

### get-style
returns current color edit mode style

## HTTP

### /miniblock
Http stream that sends a "miniblock" of data 36 times a second.
```
pub struct MiniBlock {
    pub time: f32,
    pub daggers_fired: i32,
    pub daggers_hit: i32,
    pub enemies_alive: i32,
    pub gems_collected: i32,
    pub gems_despawned: i32,
    pub gems_eaten: i32,
    pub gems_total: i32,
    pub homing: i32,
    pub kills: i32,
}
```