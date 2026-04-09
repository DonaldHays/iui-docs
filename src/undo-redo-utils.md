# Utilities for Undo/Redo Support
IUI does not have any direct support for undo/redo managers. Undo/redo support is a valuable part of any editing application, but it's primarily driven at the data/[model](https://en.wikipedia.org/wiki/Model–view–controller#Model) layer, not the UI layer, so it's mostly out of scope for IUI.

However, IUI does supply a pair of optional callbacks, `iui.widgetActivated` and `iui.widgetDeactivated`, which help manage one particularly tough detail of undo managers.

Naïvely, an undo operation should be created every time the user changes the model. However, some controls—like [sliders](./iui-slider.md)—may cause many value changes in a single user interaction. If the user grabs a slider and drags it to some desired position, the slider will pass through many intermediate positions along the way, changing the model at each step. If you created an undo operation for every little change, you may produce many distinct undo operations for a single slider interaction, which will be frustrating for the user.

`iui.widgetActivated` and `iui.widgetDeactivated` signal when control interaction sessions begin and end. If you implement these callbacks, you can use those signals to batch operations into a single undo group.

```lua
local sliderValue = 50

local function updateUI()
	local oldValue = sliderValue
	sliderValue = iui.slider("Slider", sliderValue, 0, 100)
	
	if sliderValue ~= oldValue then
		print("Value Changed")
	end
end

function iui.widgetActivated()
	print("Begin Undo Group")
end

function iui.widgetDeactivated()
	print("End Undo Group")
end
```

