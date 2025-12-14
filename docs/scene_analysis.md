# Scene Analysis Triggers

The **Scene Analysis Triggers** feature lets you define triggers via metadata coming from Axis Scene Description, and react whenever an object crosses a line or enters/leaves a area.

## Live preview and overlays

The central card shows the live view component. An overlay layer sits on top of the video and renders:

- polygonal **area triggers** (green outlines) and **direction markers** for lines so you can double-check the crossing direction.
- bounding boxes for every metadata observation, including colored corners and text that summarizes the class, score, line name, and area names.
- line segments (dashed strokes when editing) that you can drag to change the start/end points and direction (`enter`, `exit`, or both).

Use the **Add area trigger** and **Add line trigger** buttons below the stream to drop new shapes. Click the pencil/delete/tag icons for each control bubble to edit the polygon or assign a label. The small dialog that pops up is the same one used for names anywhere else in Flow—only letters, digits, spaces, and underscores are allowed.

## Filtering the metadata feed

The form on the left lets you tune the metadata that actually ends up in the zone:

- **Metadata Channel**: Choose which metadata channel (1–4) to listen to.
- **Score filtering**: Toggle the minimum-score filter and slide the threshold (0–1). Objects below the threshold are still visible but are marked in grey and will not trigger area/line colors.
- **Class filtering**: Restrict the stream to a preset list (`Human`, `Car`, `Truck`, `Bus`, `LicensePlate`). You can check multiple classes at once.


## Geometry helpers

- Use the area or line handles to drag vertices (left click) or delete them (right click). Holding the pointer inside an area lets you move the whole area.
- When no points exist, clicking on the overlay creates a default square around the cursor and immediately enters edit mode, so you can `Add area trigger` and draw in one flow.
- Direction markers annotate each configured line, plus the overlay controls let you show/hide the area/line helpers (`Hide Areas & Lines`) or toggle the metadata bounding boxes (`Hide Bounding Boxes`). Turning off the geometry hides the handles but keeps the definitions alive.

