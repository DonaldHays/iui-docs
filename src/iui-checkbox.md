# iui.checkbox
A widget that toggles boolean values.

```lua
newValue = iui.checkbox(name, currentValue)
```
### Arguments

| Name           | Type      | Description                                                   |
| -------------- | --------- | ------------------------------------------------------------- |
| `name`         | `string`  | The [ID](./identifiers.md). Displayed as the checkbox's text. |
| `currentValue` | `boolean` | The existing value for the checkbox.                          |
### Returns

| Name       | Type      | Description                     |
| ---------- | --------- | ------------------------------- |
| `newValue` | `boolean` | The new value for the checkbox. |
## Overview
If the user presses a checkbox, `newValue` will be the inverse of `currentValue`, otherwise it'll be the same. Typically, you'll have one boolean variable that you both pass to the method and assign its return value to.

```lua
local value = true

local function updateUI()
	value = iui.checkbox("Some Checkbox", value)
end
```