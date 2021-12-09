# useful feet stuff

**tl;dr:** some _very_ basic macros, templates, and other required config for my
[USB foot pedal setup](https://www.gamingmouse.com/gaming/fragpedal/quad/) &
software.

## what??

_foot pedals_. buttons for your feet. pretty wild stuff.

in most cases i configure them as simple generic bindings, where each pedal is
bound to a different function key (F8 -> F11). each key is just bound to context
context specific actions ex. push-to-talk for comms, holding breath and toggling
sights in Tarkov, etc. i have some more specific setups for things like
[flask pianoing](./FlaskPiano.idi) in Path of Exile, and
[spamming left click](./RepeatLeftClick.idi) when it's called for.

they're useful things. highly recommend.

## usage

1. install the GWS IDI software
2. `git clone git@github.com:meatwallace/useful-feet-stuff.git somewhere/`
3. `mv somewhere/{*,.*} C:\Users\Public\Public Hardware\GWS\IDI Device\macro/`

## scripting notes

### pressing & releasing keys via `KeyActionHid`

`KeyActionHid` is used for basic key press lifecycle management. it's function
signature looks like:

```
// pseudo-code
KeyActionHid(modifier: ModifierHexcodeMask, key: KeyHexcode, action: Action)
```

- key codes are mapped to a hexcode ID specific to USB 1.11 from what i can tell
- they can be found at
  [this helpful gist](https://gist.github.com/MightyPork/6da26e382a7ad91b5496ee55fdc73db2).
  a copy of the above gist is inlined in the repo
  [here](./reference/usb_hid_keys.h) for posterity.

#### `ModifierMaskHexcode`

the `modifier` arg maps to the above hexcodes reference specifically for
modifier masks.

NOTE: it may not be limited to the subset listed below, however these are the
only ones creatable via the macro builder UI. diverging from these (ex. using
right side modifier keys) is entirely untested.

- `0x00` -> NONE
- `0x01` -> CTRL
- `0x02` -> SHIFT
- `0x04` -> ALT
- `0x08` -> META (or "GUI" in GWS IDI UI)

#### `KeyHexcode`

the `key` arg should be resolved using the above hexcode reference.

#### `Action`

the `action` arg includes values to create a `press -> release` lifecycle as
well as another code for the entire lifecycle in one action:

- `0` -> press
- `1` -> release
- `30` -> complete key press

#### example usage:

```
//  (ALT, F4, press)
KeyActionHid(0x04,0x42,1)
```
