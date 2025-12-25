# Input & Output Nodes

Nodes for manual data entry, constant values, and visual displays.

---

## Input Nodes

Input nodes provide constant values or manual controls that you configure in the properties panel.

### Input Bool

Outputs a configurable boolean value.

**Category**: Input

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | The configured true/false value |

#### Properties

- **Value**: Toggle between true and false

#### Use Cases

- Permanent enable/disable signals
- Test conditions without wiring
- Default values for composites

---

### Input Int

Outputs a configurable integer value.

**Category**: Input

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | The configured number |

#### Properties

- **Value**: Enter any whole number

#### Use Cases

- Threshold values for comparisons
- Counter presets
- Index values for multiplexers
- Configuration constants

---

### Input Float

Outputs a configurable floating-point value.

**Category**: Input

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Float | The configured decimal number |

#### Properties

- **Value**: Enter any decimal number (e.g., 3.14, -0.5, 100.0)

#### Use Cases

- Temperature thresholds
- Calibration values
- Mathematical constants
- Percentage values

---

### Input String

Outputs a configurable text string.

**Category**: Input, String

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | String | The configured text |

#### Properties

- **Value**: Enter any text

#### Use Cases

- Static messages for notifications
- Device identifiers
- URL parts for HTTP requests
- File paths for SD card operations
- JSON templates

---

### Enable Input

A special input that always outputs `true`. Simpler than Input Bool when you just need a permanent enable signal.

**Category**: Input

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| True | Boolean | Always true |

---

### Button

A momentary switch that outputs `true` while pressed.

**Category**: Action, Input

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Active | Boolean | True while pressed, false when released |

#### Use Cases

- Manual trigger for testing
- Operator controls
- Reset buttons
- Acknowledge alarms

---

## Display Nodes

### Seven Segment Display

Displays a numeric value in classic seven-segment style, like a digital clock or industrial display.

**Category**: Utility

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | String | Number to display (as text) |

#### Properties

- **Digits**: Number of display digits
- **Color**: Segment color
- **Background**: Display background color

#### Display Format

The node shows digits 0-9 in the familiar seven-segment pattern:

```
 ▄▄▄     ▄▄▄   ▄▄▄         ▄▄▄   ▄▄▄   ▄▄▄   ▄▄▄   ▄▄▄
█   █      █      █  █   █  █      █      █      █  █   █  █   █
          ▄▄▄   ▄▄▄   ▄▄▄   ▄▄▄   ▄▄▄         ▄▄▄   ▄▄▄
█   █      █  █         █      █  █   █      █  █   █      █
 ▄▄▄     ▄▄▄   ▄▄▄         ▄▄▄   ▄▄▄         ▄▄▄   ▄▄▄
  0       1     2     3     4     5     6     7     8     9
```

#### Use Cases

- Counter displays
- Sensor readings
- Status codes
- Timer displays

---

### Speedometer

Displays a numeric value as a gauge dial.

**Category**: Output, Utility

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Float | Number to display on gauge |

#### Properties

| Property | Description |
|----------|-------------|
| Min | Minimum scale value |
| Max | Maximum scale value |
| Segments | Number of colored sections |
| Needle Color | Color of the pointer |
| Start Color | Color at minimum end |
| End Color | Color at maximum end |
| Value Format | Decimal places (`.0f`, `.1f`, `.2f`) |

#### Use Cases

- Speed displays
- Temperature gauges
- Load indicators
- Percentage meters

---

### Oscilloscope

Plots up to five signals on a scrolling chart over time.

**Category**: Output, Utility

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Input 1-5 | Bool/Int/Float | Signals to plot |

#### Properties

| Property | Description |
|----------|-------------|
| Colors | Line color for each input |
| Min/Max | Y-axis range for each input |
| Data Type | Type of each input (bool/int/float) |

#### Use Cases

- Signal debugging
- Timing analysis
- Waveform monitoring
- Comparing multiple values

---

### Text

Displays static text on the canvas.

**Category**: Utility

#### Properties

| Property | Description |
|----------|-------------|
| Text | The message to display |
| Size | Font size |
| Color | Text color |

#### Use Cases

- Section labels
- Instructions
- Node group titles
- Documentation notes

---

## Debug Output Nodes

### Bool Debug Output

Shows a boolean value with customizable appearance.

**Category**: Output, Debug

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | State to display |

#### Properties

| Property | Description |
|----------|-------------|
| True Icon | Icon when true |
| False Icon | Icon when false |
| True Color | Color when true |
| False Color | Color when false |
| True Label | Text when true (e.g., "ON") |
| False Label | Text when false (e.g., "OFF") |

---

### String Debug Output

Shows the current string value.

**Category**: Output, Debug

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | String | Text to display |

---

### Value Debug Output

Shows multiple input types in a table format.

**Category**: Output, Debug

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value - Bool | Boolean | Boolean value |
| Value - Int | Integer | Integer value |
| Value - Float | Float | Decimal value |
| Value - String | String | Text value |

---

### JSON Viewer

Displays formatted JSON with syntax highlighting.

**Category**: Output, Debug, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| JSON | String | JSON text to format |

#### Properties

| Property | Description |
|----------|-------------|
| Indent | Spaces for indentation |
| Height | Display height |
| Theme | Color scheme |

---

### Logging

Shows runtime log messages from the Flow engine.

**Category**: Output, Debug

Automatically receives log messages including errors, warnings, and info. Each entry shows:

- Log level (error/warning/info)
- Timestamp
- Node name/ID
- Message content

!!! tip
    Keep a Logging node in your flow to see backend messages while debugging.

---

## HTTP Request (Detailed)

The HTTP Request node makes web requests and handles responses.

**Category**: Action

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge sends request |
| Reset | Boolean | Cancels pending request |
| Body | String | Request body (overrides configured body) |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Success | Boolean | True if request succeeded (2xx status) |
| Status | Integer | HTTP status code (200, 404, 500, etc.) |
| Body | String | Response body text |
| Error | String | Error message if request failed |
| Executed | Boolean | Pulses true when request completes |
| Image ID | String | Reference if response is a JPEG image |

### Properties

| Property | Description |
|----------|-------------|
| URL | Full URL including protocol (https://...) |
| Method | GET, POST, PUT, DELETE, PATCH |
| Body Type | none, form, json, raw, xml |
| Body | Request body content |
| Headers | Custom HTTP headers |
| Params | URL query parameters |
| Timeout | Request timeout in seconds |
| Skip TLS Verify | Ignore certificate errors |

### Authentication

| Type | Description |
|------|-------------|
| None | No authentication |
| Basic | Username/password (base64 encoded) |
| Digest | Challenge-response authentication |

### Examples

#### GET Request
```
URL: https://api.example.com/status
Method: GET
```

#### POST with JSON Body
```
URL: https://api.example.com/events
Method: POST
Body Type: json
Body: {"event": "motion", "camera": "entrance"}
```

#### POST with Dynamic Body
Wire a string to the `Body` input to override the configured body dynamically.

#### Custom Headers
```
Authorization: Bearer your-token-here
Content-Type: application/json
X-Custom-Header: value
```

#### Query Parameters
```
URL: https://api.example.com/search
Params:
  - q: searchterm
  - limit: 10
```
Results in: `https://api.example.com/search?q=searchterm&limit=10`

### Handling Image Responses

When the response is a JPEG image, the node stores it in the image cache and outputs an `Image ID`. Connect this to a JPEG Viewer node to display the image.

```
[HTTP Request] ─→ Image ID ─→ [JPEG Viewer]
```

### Error Handling

| Status | Meaning |
|--------|---------|
| 200-299 | Success |
| 400 | Bad request (check your body/params) |
| 401/403 | Authentication failed |
| 404 | URL not found |
| 500 | Server error |
| 0 | Network error (check connectivity) |

!!! tip
    Connect the Error output to a Logging node to see detailed error messages.

### Sending Camera Snapshots

To send a camera image via HTTP:

1. Add **Image Snapshot** node to capture from camera
2. Configure HTTP Request with your upload URL
3. Use the Image ID from the snapshot in your request

The HTTP node can reference cached images when sending to external services.
