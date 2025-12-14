# Scene Analysis Triggers

The **Scene Analysis Triggers** feature (backend route `/metadata_trigger`) lets you draw ROI shapes directly on top of a live video stream, filter the metadata coming from Axis Scene Description or Scene Object Analytics, and react whenever an object crosses a line or enters/leaves a polygon. Unlike Flow nodes, this is a dedicated feature page under the **Emitters** section of the UI.

## Live preview and overlays

The central card shows the live `VdoLiveStream` component. An SVG layer sits on top of the video and renders:

- polygonal **area triggers** (green outlines) and **direction markers** for lines so you can double-check the crossing direction.
- bounding boxes for every metadata observation, including colored corners and text that summarizes the class, score, line name, and area names.
- line segments (dashed strokes when editing) that you can drag to change the start/end points and direction (`enter`, `exit`, or both).

Use the **Add area trigger** and **Add line trigger** buttons below the stream to drop new shapes. Click the pencil/delete/tag icons for each control bubble to edit the polygon or assign a label. The small dialog that pops up is the same one used for names anywhere else in Flow—only letters, digits, spaces, and underscores are allowed.

## Filtering the metadata feed

The form on the left lets you tune the metadata that actually ends up in the zone:

- **Metadata Channel**: Choose which metadata channel (1–4) to listen to; the backend automatically subscribes via `metadata_trigger`.
- **Score filtering**: Toggle the minimum-score filter and slide the threshold (0–1). Objects below the threshold are still visible but are marked in grey and will not trigger area/line colors.
- **Class filtering**: Restrict the stream to a preset list (`Human`, `Car`, `Truck`, `Bus`, `LicensePlate`). You can check multiple classes at once.

Any change sets `unsaved_data` so you do not lose the configuration when switching cameras or refreshing the page. Press **Save** at the bottom to persist the trigger definitions.

## Geometry helpers

- Use the SVG handles to drag vertices (left click) or delete them (right click). Holding the pointer inside an ROI lets you move the whole polygon.
- When no points exist, clicking on the overlay creates a default square around the cursor and immediately enters edit mode, so you can `Add area trigger` and draw in one flow.
- Direction markers annotate each configured line, plus the overlay controls let you show/hide the area/line helpers (`Hide Areas & Lines`) or toggle the metadata bounding boxes (`Hide Bounding Boxes`). Turning off the geometry hides the handles but keeps the definitions alive.

## Observation stream

The feature keeps a WebSocket open at `/ws/metadata_trigger`. Each message is expected to be an array of observations with properties such as `bounding_box`, `class`, `score`, `line_crossed`, `line_direction`, `inside_areas`, and `crossed_lines`.

- Boxes are drawn with different colors depending on whether the track is touching a line, inside an area, or simply a valid detection. Highlighted labels summarize the class, confidence, and the name of the crossed line/area.
- The `boxshow` toggle stops rendering these rectangles without closing the socket (the data is still stored in memory for the live overlays).
- If the video stream disconnects, the overlays are cleared automatically, and reconnects are retried via the `streamConnected` watcher.

## Tips

- Feed the recorded metadata into Flow by wiring metadata event subscribe nodes; they use the same MDB stream that this feature visualizes.
- Saved area/line triggers are stored on the backend and shared across restarts. You can hook them into other features by adding `AddAreaEnterHandler`/`AddLineCrossHandler` callbacks in Go via `MetaDataTriggerFeature`.
- Use this UI to prototype rules before copying the geometry into Flow nodes or storing the IDs for downstream automations.
