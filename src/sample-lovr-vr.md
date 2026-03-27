# LÖVR - VR
## conf.lua
```lua
function lovr.conf(t)
    t.headset.supersample = 2
end
```
## main.lua
```lua
local iui = require "iui"
local backend = require "lovr-iui"

--- @type LovrIUIWorldWindow
local window

local labelText = "Click the button!"

function lovr.load()
    iui.load(backend)

    window = backend.worldWindow.new()
end

function lovr.update(dt)
    iui.beginFrame(dt)

    if window:beginFrame() then
        iui.panelBackground()
        iui.label(labelText)
        if iui.button("Say Hello") then
            labelText = "Hello, World!"
        end

        window:endFrame()
    end

    iui.endFrame()
end

function lovr.draw(pass)
    window:draw(pass)
end
```