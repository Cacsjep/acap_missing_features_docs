# Device Integration Nodes

Nodes for interacting with physical Axis devices, streams, and system events.

## Axis RTSP Stream
The **Axis RTSP Stream** node displays a live video stream from an Axis device and can overlay metadata.

**Category**: Device
**Type**: `AxisRtspStreamNode`

### Functionality
Connects to a device's RTSP stream. Supports visualizing region-of-interest (ROI) and object detection metadata overlays.

### Inputs
- **ROI** (JSON): Region of Interest polygon points (if overlay enabled).
- **Observations** (JSON): Object detection data to draw bounding boxes.
- **Tripwire** (JSON): Tripwire line definition.

### Configuration
1.  **Device**: Select the target Axis device from the dropdown property.
2.  The node will attempt to connect and display the stream automatically when the flow is "Live".

---

## Axis SVG Overlay

The **Axis SVG Overlay** node draws an SVG graphic onto the camera image. The SVG is a small template, and any `{{ name }}` placeholder you put inside it becomes an input on the node, so values from your flow (sensor readings, statuses, counters) can change the picture live.

**Category**: Device
**Type**: `AxisSVGOverlayNode`

**Before you start**: enable the overlay service in **Settings → Overlay**. Without it, nothing will be drawn on the stream.

### Inputs

| Name | Type | Description |
|---|---|---|
| **Enable** | Boolean | Turns the overlay on or off live. When false, the overlay is cleared from the stream. |
| *(your variables)* | Any | One input per `{{ name }}` placeholder in your SVG. Inputs appear automatically when you save the template. Pick the data type for each one from the dropdown. |

### Properties

| Field | Description |
|---|---|
| **Stream** | Which camera stream to draw on (0 = main stream). |
| **Anchor** | Where the SVG sits on the image: `top-left`, `top-right`, `bottom-left`, `bottom-right`, or `center`. Used when your SVG declares a `width` and `height`. |
| **SVG Editor** | Opens a full-screen editor with the SVG on the left and a live preview on the right. |

### SVG Editor

Click **SVG Editor** to open the full-screen view. You edit the SVG on the left and watch the preview on the right. Two preview modes:

- **Literal**: shows your `{{ name }}` placeholders as text, so you can check the layout and shapes.
- **Sample values**: fills the placeholders with sample numbers so the overlay actually draws something.

The **Beautify** button above the editor (next to the variable counter) re-indents the template. It keeps `{{ }}` expressions, `{{#each}}` / `{{#if}}` blocks and attribute values exactly as written and indents loop / conditional bodies, so it is safe to run on a template at any time.

The right side also has a help section. The first panel, **LLM prompt: generate SVG with an AI assistant**, gives you a ready-made prompt you can paste into ChatGPT, Claude, or any other AI assistant. Type what overlay you want at the bottom of the prompt and the assistant will generate an SVG that works here. The other panels list the template syntax and supported SVG features.

### Visual editor

The preview is also an editor. You don't have to write XML by hand:

- **Insert shapes**: the small toolbar above the preview adds a rectangle, circle, ellipse, line, or text element at the center of the canvas.
- **Select**: click any shape in the preview to select it. A Properties panel opens on the right showing its attributes.
- **Edit properties**: change values with typed fields - number boxes for sizes/positions, dropdowns for choices like `text-anchor` or `font-weight`, and colour swatches for `fill` and `stroke` (the border colour). You can also edit a text element's content and use **Add property** to introduce a common attribute the shape doesn't have yet.
- **Move and resize**: drag a selected shape to move it, or drag its handles to resize (lines have endpoint handles).
- **Reorder**: bring-to-front / send-to-back buttons change which shape draws on top (SVG paints later elements over earlier ones).
- **Duplicate / delete**: buttons in the Properties panel, or press **Delete** to remove the selected shape.

Everything you do in the visual editor writes back into the SVG source as a minimal change, so it never disturbs your `{{ }}` expressions or `{{#each}}` / `{{#if}}` blocks. Elements that are inside a loop or whose values come from an expression are shown read-only in the panel - edit those in the source on the left.

### Placeholders

Anywhere in the SVG you can write `{{ name }}` to insert a value from a node input. You can do simple math and comparisons inside the braces too:

- Numbers, text, true / false
- `+ - * / %` for math
- `<  >  ==  !=` for comparing
- `cond ? a : b` to pick between two values (used a lot for colour changes)

You can also wrap whole parts of the SVG with an if/else block:

```xml
{{#if alert}}
  <circle cx="20" cy="20" r="6" fill="red"/>
{{else}}
  <circle cx="20" cy="20" r="6" fill="green"/>
{{/if}}
```

!!! tip "Special characters inside { { } }"
    If you need `<`, `>` or `&` inside an attribute expression, write `&lt;`, `&gt;` and `&amp;` instead. This works the same inside `{{ }}`, `{{#if}}` and `{{#each}}`. If you accidentally paste an SVG whose **tags** are escaped (e.g. `&lt;rect&gt;` instead of `<rect>`, common when copying from an AI assistant), the preview shows a clear error with the affected lines and a **Fix automatically** button.

### Loops

To draw a list, repeat a block once per element of a JSON array with `{{#each}}`:

```xml
{{#each matches as m, i}}
  <g transform="translate(0 {{ i * 80 }})">
    <text x="20" y="40">{{ m.homeTeam.name }}</text>
    <text x="200" y="40">{{ m.score.fullTime.home }} - {{ m.score.fullTime.away }}</text>
  </g>
{{/each}}
```

- The collection (`matches`) is a **single input port that holds a JSON array as a string** - set its data type to **String**. Flow transmits JSON as text, so feed the array text straight in.
- `as m` names each element; the optional `, i` adds the 0-based index. Both are loop-local and do **not** become input ports.
- Reach into each element with dot paths: `{{ m.homeTeam.name }}`, `{{ m.score.fullTime.home }}`. Missing fields render empty.
- SVG has no automatic layout, so position each repeat yourself using the index, e.g. `translate(0 {{ i * 80 }})` or `y="{{ 40 + i * 40 }}"`.
- Loops may nest and may contain `{{#if}}` blocks. The `<svg>` canvas size is fixed, so size it for the expected number of rows.

!!! note "Text whitespace"
    Inside `<text>` and `<tspan>`, line breaks and runs of spaces are collapsed to a single space (standard SVG). Indent the markup freely, but for alignment use coordinates (`x` / `dx`), not extra spaces.

### Images

You can draw a raster image (PNG or JPEG) with `<image>`. The source can be a base64 `data:` URI or a remote `http(s)` URL:

```xml
<!-- Remote URL (fetched by the device) -->
<image x="20" y="20" width="64" height="64" href="{{img_url}}"/>

<!-- Inline base64 (no network needed) -->
<image x="20" y="20" width="64" height="64"
       href="data:image/png;base64,iVBORw0KGgoAAA..."/>
```

- `x` / `y` / `width` / `height` accept `{{ }}` expressions. If `width` or `height` is `0`, the image's native pixel size is used.
- The `href` can itself be an expression, e.g. `href="{{ item.crest }}"` inside a `{{#each}}` loop, so each row can show a different image.
- **Supported formats: PNG and JPEG.**
- Decoded images are cached on the device, so reusing the same image across frames or rows is cheap.

!!! note "How remote images load"
    Remote images are fetched in the background so the overlay never stalls waiting for the network. The first time a URL appears the image is downloaded; it shows up a moment later once it arrives, and nothing is drawn for that image until then. A URL is fetched **once** and cached - feed a different URL (for example a new value into `{{img_url}}`) to make it re-fetch. The same URL is never re-downloaded on its own.

!!! warning "HTTPS and self-signed certificates"
    Remote images are fetched over a standard HTTPS client that validates certificates, so a server using a self-signed or private-CA certificate will fail to load by default. If you need to load images from such a server, enable **Allow untrusted TLS certificates for remote images** in **Settings → Overlay**. Leave it off unless you need it - it disables certificate validation for those fetches.

### Sizing

Two ways the SVG can fit on the camera image:

| Mode | When | Result |
|---|---|---|
| **Fixed size** | Your `<svg>` has a `width` and `height` | The SVG is drawn at exactly that size and placed at the selected **Anchor** corner / centre. The rest of the image stays transparent. |
| **Full stream** | No `width` and `height` (only `viewBox`) | The SVG is stretched to cover the whole camera image. |

Compact sizes (for example 360×360 or 240×520) usually look best. Pick a corner with **Anchor** and leave the rest of the picture visible.

### Example

A vertical bar that fills as a `level` value (0–100) goes up, switching colour from green to amber to red:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 60 300" width="60" height="300">
  <style>
    .track { fill:#1a1d24; stroke:#2d3340; stroke-width:1 }
    .label { font:500 10px "DejaVu Sans"; fill:#cfd2db }
  </style>
  <rect class="track" x="20" y="40" width="20" height="240" rx="10"/>
  <rect x="20"
        y="{{ 280 - level * 2.4 }}"
        width="20"
        height="{{ level * 2.4 }}"
        rx="10"
        fill="{{ level &lt; 20 ? '#ef5350' : level &lt; 80 ? '#f8d54b' : '#8fcb6e' }}"/>
  <text class="label" x="30" y="20" text-anchor="middle">{{ level }}%</text>
</svg>
```

Connect a number (a sensor reading, a counter, anything) to the `level` input. Leave **Enable** turned on. The bar updates live on the camera image.

---

## Axis Output
The **Axis Output** node controls physical I/O ports on the device (e.g., relays, digital outs).

**Category**: Device
**Type**: `AxisOutputNode`

### Functionality
Sets the state of a physical port based on the input signal.

### Inputs
- **In 1, In 2, ...**: Boolean inputs corresponding to the device's ports.

### Configuration
-  **Port Mapping**: Ports are typically mapped when adding the node or via the device selection dialog.

---

## Metadata Event Create
The **Metadata Event Create** node defines a custom event that can be emitted by the device.

**Category**: Device
**Type**: `MetaDataEventCreateNode`

### Functionality
Constructs a structured event payload. This can be used to trigger rules on the device or be consumed by other services.

### Properties
- **Event Name**: Unique identifier for the event.
- **Event Type**:
  - **Stateless** (Trigger): Pulse-like event (e.g., "Motion Detected").
  - **Stateful**: Persistent state (e.g., "Door Open").

### Inputs
- **Custom Data fields**: Add fields via the "+" button in properties. You can define them as:
  - **Source**: Origin of the event.
  - **Data**: Payload value.

---

## Metadata Event Subscribe
The **Metadata Event Subscribe** node listens for system or custom events.

**Category**: Device
**Type**: `MetaDataEventSubscribeNode`

### Functionality
Fires outputs when a specific event occurs on the device (e.g., specific MQTT topic or internal rule).

### Configuration
- **Setup**: When adding this node, a dialog typically appears to select the specific event topic you wish to subscribe to.
- **Outputs**: Dynamic outputs are created based on the event's data fields.

---

## Analytics Metadata

The **Analytics Metadata** node receives raw analytics metadata from the device's built-in object detection (AXIS Object Analytics).

**Category**: Device
**Type**: `AnalyticsMetadataNode`

### Functionality

Streams real-time object detection data from the device's analytics engine. Supports two modes for different use cases.

### Modes

| Mode | Description |
|------|-------------|
| **Scene Description** | Real-time frames with current observations (objects in frame) |
| **Consolidated Track** | Complete track data after an object leaves the scene |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| New Data | Boolean | Pulses true when new metadata arrives |
| JSON | String | Raw JSON containing observations or track data |
| Error | String | Error message if streaming fails |

### Properties

- **Mode**: Select Scene Description or Consolidated Track
- **Framerate**: How often to poll for new data (frames per second)

### Scene Description JSON Example

```json
{
  "observations": [
    {
      "id": "track_123",
      "type": "Human",
      "boundingBox": {"x": 0.2, "y": 0.3, "width": 0.1, "height": 0.3},
      "confidence": 0.95
    }
  ]
}
```

!!! tip
    Use Scene Description for real-time monitoring and triggering. Use Consolidated Track for logging complete object paths after they leave the frame.

---

## License Plate List
The **License Plate List** node checks a plate string against a configured local database.

**Category**: Device / ANPR
**Type**: `LicensePlateListNode`

### Functionality
Verifies if a license plate exists in the internal list and checks its validity (expiration, allowed time windows).

### Inputs
- **Process** (Boolean): Triggers the lookup.
- **Plate** (String): The text to check.
- **Reset** (Boolean): Clears outputs.

### Outputs
- **Matched Plate** (String): The best match found.
- **In List** (Boolean): True if found.
- **Not In List** (Boolean): True if not found.
- **Expired** (Boolean): True if found but license is expired.
- **Not Valid Yet** (Boolean): True if valid from current time > now.
- **Tag outputs**: Boolean outputs for each tag (e.g., "Staff", "VIP") associated with the plate.

!!! note "Tag output updates"
    Tag outputs are created when you add the node to your flow based on the tags that exist at that time. If you add new tags to the License Plate List feature later, you must **delete and recreate** the node to get outputs for the new tags. Plate data updates are reflected immediately without recreating the node.

---

## Vaxtor Cloud ANPR
The **Vaxtor Cloud ANPR** node sends an image to the Vaxtor Cloud for recognition.

**Category**: Device / ANPR
**Type**: `VaxtorCloudAnprNode`

### Functionality
Captures an image from the connected device and uploads it to the Vaxtor Cloud API for analysis.

### Inputs
- **Execute** (Boolean): Triggers the capture and upload.
- **Reset** (Boolean): Cancels pending request.

### Outputs
- **Success** (Boolean): API request succeeded.
- **Plate Found** (Boolean): Cloud returned a plate.
- **Number / Country / State** (String): Plate details.
- **Vehicle Make / Model / Color** (String): Vehicle attributes.
- **Status** (Integer): HTTP status code (200=OK) or internal error code.

### Configuration
- **Authentication**: Uses configured client credentials to obtain a token from `auth.vaxtor.cloud`.
