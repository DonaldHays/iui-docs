# Layout
The layout engine is built around two key concepts: panels and rows. Panels are rectangular regions within a window. Rows are placed within panels. The layout engine places widgets inside rows inside panels.

![Layout diagram showing windows, panels, and rows](./img/layout-panels-rows.svg)

## Panels

## Rows

## Margin, Spacing, Padding
In `iui.style`, three properties guide the layout engine, `margin`, `spacing`, and `padding`.

The `margin` insets rows within a panel, creating a gap between the edges of rows and their containing panels.

The layout engine uses `spacing` to add gaps between widgets in rows. The `spacing` also impacts the vertical space between widgets in new rows.

Finally, widgets use `padding` to try, room permitting, to inset their content within their boundaries. Additionally, `padding` is used when calculating the default row height, if you don't pass a manual height when creating a row. The default height of a row is `fontHeight + padding * 2`.