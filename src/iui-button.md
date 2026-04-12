# iui.button
A widget that performs an action when pushed.

```lua
wasPressed = iui.button(name)
```
### Arguments

| Name   | Type     | Description                                                 |
| ------ | -------- | ----------------------------------------------------------- |
| `name` | `string` | The [ID](./identifiers.md). Displayed as the button's text. |
### Returns

| Name         | Type      | Description                               |
| ------------ | --------- | ----------------------------------------- |
| `wasPressed` | `boolean` | Whether the button was pushed this frame. |
## Overview
Buttons take their label as an argument, and return `true` when they're pressed. You should invoke the button's action whenever it returns `true`. The typical code pattern for a button is to wrap it in an `if` statement.

```lua
if iui.button("Say Hello") then
	print("Hello, World!")
end
```