
# hyperex

hyperex is Hammerspoon script which provides yet another modifier key. With simplicity, adaptability, diversity.

## Install

Place `hyperex.lua` into `~/.hammerspoon/`

## Constructor

```lua
local hyperex = require('hyperex')
local hx = hyperex.new('f18') -- f18 becomes hyper-key
```

## adding shortcut and function

This setting acts like hotkey during press hyper-key.

```lua
-- syntax
-- hyperex:bind(key):to(key, {mods, ...})
-- hyperex:bind(key):to(function)

-- hyper + a -> b
hx:bind('a'):to('b')
-- hyper + ] -> shift + 8
hx:bind(']'):to('8', {'shift'})
-- hyper + z -> trigger function
hx:bind('z'):to(function() hs.eventtap.keyStroke({}, 'h') end)

```


## adding modifiers

This setting modify only modifier keys.

```
-- syntax
hyperex:mod({mods, ...}):to({key, otherKey, ...})
```

```lua
--  hyper + 3 or 4 -> cmd + shift + 3 or 4
hx:mod({'cmd', 'shift'}):to('3','4')

-- some special idiom
-- hyper + anyKey -> ctrl + shift + anyKey
hx:mod({'ctrl', 'shift'}):to('any')
-- hyper + a~z -> ctrl + shift + a~z
hx:mod({'ctrl', 'shift'}):to('atoz')
-- hyper + f1~f15 -> ctrl + shift + f1~f15
hx:mod({'ctrl', 'shift'}):to('fkeys')
```

## InitialKey and EmptyHitKey
InitialKey is triggered immediately when hyper-key is pressed.  
EmptyHitKey is triggered when hyper-key is released if any setting was not triggered.

```lua
hx:setInitialKey(0x66) --eisu
hx:setEmptyHitKey(0x68) --kana
```

## message
Show message with `hs.alert()` when triggered. Default duration is 0.4 sec.
```lua
-- syntax
-- hyperex:withMessage(message[, leaveMessage][, duration])
-- setting:withMessage(message[, duration])

local hx = hyperex.new(0x50):withMessage("hello", "bye", 0.5)
hx:bind('a'):to('a', {}):withMessage("a was pressed", 0.1)
```

## Other

hyperex can be used multiple instances.

```lua
local hyperex = require('hyperex')
local hx = hyperex.new('f18')
local hx2 = hyperex.new('f19')
```

You can not set the same key as hyper-key

```lua
local hx = hyperex.new('f18')
hx:bind('f18'):to(...) -- disabled
hx:setInitialKey('f18') -- disabled
hx:setEmptyHitKey('f18') -- disabled
hx:bind('f1'):to('f18') -- enabled but confuse
```

Give priority to later settings. `bind` takes precedence over `mod` all the time.
```lua
local hx = hyperex.new('f18')
hx:mod({'alt'}):to('f2','f3') -- only f3 is enabled
hx:bind('f1'):to(...) -- disabled
hx:bind('f1'):to(...) -- enabled
hx:mod({'ctrl', 'shift'}):to('f1','f2') -- only f2 is enabled 
```

hyperex handles only real keyboard input.
```lua
-- this setting could be triggered. infinite loop does not occur.
hx:bind('x'):to(function() hs.eventtap.keyStroke({}, 'x') end)
```
Method chain

| class   | method | return |
|---------|--------|--------|
|CHyper   |.new()|CHyper|
|CHyper   |:setInitialKey()|self|
|CHyper   |:setEmptyHitKey()|self|
|CHyper   |:bind()|CBinder|
|CHyper   |:mod()|CModifier|
|CBinder  |:to()|self|
|CModifier|:to()|self|
|*        |:withMessage()|self|



## License

WTFPL 2.0

## Author

hetima  
https://twitter.com/hetima