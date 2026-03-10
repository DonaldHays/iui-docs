# The Style Stack
To add a widget to a UI, you call a widget function and pass some arguments. These arguments typically include a name, a value, and maybe a few other things. However, widgets may potentially support *many* different customization options. To include all these options in the function signature would require lengthy argument lists. As an alternative, IUI offers `iui.style`, a table-like object.

## Basics
At its simplest, `iui.style` behaves like a table. You can assign values to keys, and widgets can retrieve those values. For example, widgets use the font stored in the `"font"` key when drawing text.

```lua
-- This label will use the default font.
iui.label("The default font!")

-- This label will use `someCustomFont`.
iui.style["font"] = someCustomFont
iui.label("A custom font!")
```

## The Stack
Changing a value in `iui.style` will apply the new value to every future widget for the rest of the frame. But sometimes you only want to customize one or a few widgets, not every future widget. To support this, `iui.style` implements scoped stack semantics, using `iui.style.push()` and `iui.style.pop()`.

Calling `push` creates a scope. Any customizations made after calling `push` will be undone after calling `pop`. Any call to `push` _must_ be balanced by a call to `pop`. Any keys that you don't customize in a scope will inherit from parent scopes.

```lua
-- This label will use the default font.
iui.label("The default font!")

-- This pushes a new scope on the style stack.
iui.style.push()

-- This label uses the default font, because we haven't made any changes yet.
iui.label("Still using the default font!")

-- These labels will use `someCustomFont`.
iui.style["font"] = someCustomFont
iui.label("A custom font!")
iui.label("Still using the custom font!")

-- This pops the scope off the stack.
iui.style.pop()

-- This label uses the default font, not `someCustomFont`.
iui.label("Back to the default font!")
```

## The Default Table
There is another table at `iui.style.default`. This table specifies default options. If you want to set a value for your entire application once, and not have to set it every frame, you can set it in `iui.style.default`. This table is set as the root of the style stack on every new frame.

## Dependency Injection
You don't *just* have to use `iui.style` for presentation-oriented values for widgets. You may freely inject and retrieve any values you want, anywhere you want. You can use this to implement the [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) technique.

For example, imagine you're making an app that asks the user to log in. If they've done so, you could inject the user object in `iui.style["user"]`. Then, anywhere else in your UI, you could try fetching the value for that key. If the user has logged in, you'll get the user object back. If they haven't, you'll get `nil`. You could use this to change elements of the UI based on whether or not they've logged in. By injecting the user object in `iui.style`, you avoid needing to manually pass it through your user interface via arguments.

The [sample project](https://github.com/DonaldHays/iui-sample) uses this pattern to inject app and window states into `iui.style`. They're injected in `sampleMain`, and then retrieved throughout the app. For example, `splitPrimaryPane` retrieves the app state. The `splitPrimaryPane` function takes no arguments, and is called several layers deep in the UI. Without injecting into `iui.style`, the state would have had to have been passed through several functions.

## Built-in Keys
**Layout**

| Key            | Default | Description                                                            |
| -------------- | ------- | ---------------------------------------------------------------------- |
| `"margin"`     | 8       | The distance between widgets and the edge of panels.                   |
| `"spacing"`    | 8       | The distance between widgets within a panel.                           |
| `"padding"`    | 8       | The suggested padding between the border of widgets and their content. |
| `"scrollSize"` | 16      | The width of horizontal scrollbars, and height of vertical bars.       |

**Drawing**

| Key      | Default                          | Description                 |
| -------- | -------------------------------- | --------------------------- |
| `"font"` | A backend-provided 12-point font | The font used to draw text. |

**Split Views**

| Key              | Default | Description                                                                                              |
| ---------------- | ------- | -------------------------------------------------------------------------------------------------------- |
| `"splitMinEdge"` | 8       | The minimum width of the left panel of horizontal split views, or top panel of vertical split views.     |
| `"splitMaxEdge"` | 8       | The minimum width of the right panel of horizontal split views, or bottom panel of vertical split views. |

**VR**

| Key                      | Default | Description                                                                                                   |
| ------------------------ | ------- | ------------------------------------------------------------------------------------------------------------- |
| `"vrWindowCornerRadius"` | 16      | The border radius of world-space windows in VR, when panel backgrounds are drawn using `iui.panelBackground`. |
