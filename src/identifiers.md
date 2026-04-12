# Identifiers

IUI requires unique identifiers to distinguish between different widgets, as it typical for immediate mode UIs. Identifiers allow IUI to know that a given widget has persisted between frames, even if other widgets were added or removed, or the widget moved to a new location in the UI. This allows IUI to manage things like focus and interaction across frames.

## Naming an Identifier
Many widgets take a `name` as their first parameter, such as [`iui.button`](./iui-button.md). Often, these names will be displayed to the user as a control's text. But, importantly, these names are also used to identify the widget to IUI. This imposes some restrictions that may be surprising.

### Uniqueness
Most importantly, **identifiers must be unique**. If two widgets have the same identifier, IUI cannot distinguish them, and surprising behaviors may result. For example, the focus system will get confused, or the user interacting with one control may activate the other. Widget identifiers must be unique even across widget types.

If two widgets use the same ID, IUI will print a warning.

```lua
if iui.button("myWidget") then
	-- Perform action
end

-- Invalid ID: the checkbox has the same ID as the button
-- IUI will print a warning like "Warning: duplicate hash for myWidget"
checkValue = iui.checkbox("myWidget", checkValue)
```

Technically, IUI identifies widgets using ID *hashes*, not identifiers themselves. This means there's a small chance that two widgets with different IDs may have an ID collision anyway. This is very unlikely in practice, but not impossible.
### Scoping
Fortunately, you do not necessarily need to guarantee ID uniqueness across your *entire* interface. Some widgets create *identifier scopes*. IDs only need to be unique within their scope. Typically, container widgets create new scopes. For example, [`iui.subMenu`](./iui-sub-menu.md) creates a new scope, and [`iui.splitView`](./iui-split-view.md) creates scopes for each side of the split.

```lua
splitValue = iui.splitView("splitter", "horiz", splitValue,
	function()
		if iui.button("aButton") then
			-- Perform action
		end
	end,
	function()
		-- This is fine! Even though both buttons have the same
		-- ID, they're in different identifier scopes
		if iui.button("aButton") then
			-- Perform action
		end
	end
)
```

### Stability
A given widget's ID should be stable across frames. If the identifier changes, IUI will believe it's a different widget.

```lua
buttonName = "Push Me"
if iui.button(buttonName) then
	-- Not good. The button's ID will change
	buttonName = "You Pushed Me"
end
```

## Implementing Identified Widgets
If you implement a new widget, you may need to identify it to IUI. To do so, take in a parameter that will serve as the identifier to the user, and pass it to `iui.beginID` to get your identifier. At the end of your widget, call `iui.endID`. Every call to begin an ID must be balanced by a call to end the ID.

```lua
local function myWidget(name)
	local id = iui.beginID(name)
	
	iui.endID()
end
```

You typically only need to generate an identifier if your widget is interactive. For example, `iui.becomeHover`, `iui.becomeFocus` and `iui.becomeActive` are all based on the current identifier. Most non-interactive widgets, like [`iui.label`](./iui-label.md), do not generate identifiers.
### Identifier Scopes
Calling `iui.beginID` opens an identifier scope, which closes when you call `iui.endID`. As mentioned earlier, identifiers only need to be unique within their scope. If you are creating a container widget, place your child content between the calls to `iui.beginID` and `iui.endID`. Since identifier scopes make collisions less likely, you may want to create an identifier scope in a container widget even if it's not interactive.

```lua
local function myContainerWidget(name, content)
	local id = iui.beginID(name)
	
	content()
	
	iui.endID()
end
```

### Advancing Layout
The ID system interfaces slightly with the [layout](./layout.md) system to deliver a small convenience: by default, `iui.endID` calls `iui.layout.advance`.

The layout system does not advance from one widget position to the next until `iui.layout.advance` is called. This is almost always desirable at the end of a widget, and most widgets have identifiers, so `iui.endID` makes this call for you.

If you're creating a non-interactive widget, you need to advance layout manually yourself. Widgets like [`iui.label`](./iui-label.md) do this.

On rare occasion, you might not want an identified widget to advance the layout. For example, [`iui.listView`](./iui-list-view.md) embeds an [`iui.scrollView`](./iui-scroll-view.md) widget, and the scroll view advances the layout. If the list view advanced the layout, it would cause the layout to advance *twice*, when it should only do so once. To support cases like this, `iui.endID` takes an optional `advanceLayout` boolean parameter. Passing `false` as an argument will prevent `iui.endID` from calling `iui.layout.advance`.