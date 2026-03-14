# Layout
The layout engine is built around two key concepts: panels and rows. Panels are rectangular regions within a window. Rows are placed within panels. The layout engine places widgets inside rows inside panels.

![Layout diagram showing windows, panels, and rows](./img/layout-panels-rows.svg)

## Panels
A panel is a 2D rectangle in a window. You can create a panel at any time by calling `iui.layout.beginPanel(x, y, w, h, margin?)`. If you begin a panel, you must end it later by calling `iui.layout.endPanel(advance?)`. When you begin a panel, it becomes the current panel, and when you end it, the previous panel becomes the current panel again. The current panel is used to lay out rows.

It's common to create child panels within a panel. For example, the split view widget fills the current panel, and creates two sub-panels: one for each side of the split. You may then place widgets within the sub-panels, or even create further panels inside.

Even though there's a conceptual parent-child hierarchy, panels may be placed anywhere in a window, even outside their parent panel. Typically, though, you'll want to base a panel's frame on its parent's. You can get the bounds of the current panel by calling `iui.layout.getPanelBounds(index?)`, which returns the `x`, `y`, `w`, and `h` of a panel. You may then use those values as a basis for the new bounds you pass to `beginPanel`.

## Rows

## Margin, Spacing, Padding
In [`iui.style`](./style_stack.md), three properties guide the layout engine, `margin`, `spacing`, and `padding`.

![Layout diagram showing margin, spacing, and padding](./img/layout-margin-spacing-padding.svg)

The `margin` insets rows within a panel, creating a gap between the edges of rows and their panels.

The layout engine uses `spacing` to add gaps between widgets in rows. The `spacing` also applies to the vertical space between rows.

Finally, widgets use `padding` to try, room permitting, to inset their content within their boundaries. Additionally, `padding` is used when calculating the default row height, if you don't pass a manual height when creating a row. The default height of a row is `fontHeight + padding * 2`.