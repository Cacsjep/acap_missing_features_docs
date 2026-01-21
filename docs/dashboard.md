# Dashboard

The Dashboard feature provides a customizable monitoring interface where you can visualize Flow node data using various widgets. Create real-time displays for operators without exposing the underlying Flow complexity.

---

## Overview

Dashboards display live data from your Flow nodes through configurable widgets. Each widget connects to a specific node input or output and updates in real-time.

**Key Features:**

- **Multi-dashboard support** with browser-like tabs
- 17 widget types for different visualization needs
- Drag-and-drop widget placement
- Real-time data updates via WebSocket
- Edit and Live modes
- Fullscreen mode for dedicated displays
- Permission-based access control

---

## Getting Started

### Opening the Dashboard

1. Navigate to **Flow Dashboard** from the main menu
2. The dashboard loads in **Live Mode** by default

### Toolbar

The dashboard toolbar provides quick access to all controls:

```
[Tab 1] [Tab 2] [+] | [Add Widget] | [Live/Edit] [Fullscreen]
```

| Element | Description |
|---------|-------------|
| **Tabs** | Switch between dashboards |
| **+ Button** | Add a new dashboard (Edit Mode only) |
| **Add Widget** | Open widget creation dialog |
| **Live/Edit** | Toggle between modes |
| **Fullscreen** | Enter fullscreen view |

### Switching Modes

| Mode | Description |
|------|-------------|
| **Live Mode** | View real-time data, no editing possible |
| **Edit Mode** | Add, configure, and arrange widgets |

Click the **Live/Edit** button to toggle modes (requires `feature_flow_dashboard_update` permission).

---

## Multi-Dashboard Support

Create multiple dashboards to organize your monitoring views. Each dashboard has its own independent set of widgets.

### Managing Dashboards

| Action | How To |
|--------|--------|
| **Switch Dashboard** | Click on a tab |
| **Add Dashboard** | Click the **+** button (Edit Mode) |
| **Rename Dashboard** | Double-click the tab name, or click the pencil icon on the active tab |
| **Delete Dashboard** | Click the **X** button on a tab (only visible when multiple dashboards exist) |

!!! tip
    Use separate dashboards for different purposes: one for live monitoring, another for historical data, and another for specific zones or devices.

---

## Adding Widgets

1. Click **Add Widget** in Edit Mode
2. Select a widget type from the dropdown
3. Choose the **Source Node** from your Flow
4. Select **Port Type** (Input or Output)
5. Select the specific **Port**
6. Click **Add** - the widget appears on the canvas

The widget automatically finds an empty space on the dashboard.

---

## Widget Types

### Text Widget

Displays any node value as text.

**Best for:** Labels, status messages, string outputs

**Settings:**

| Option | Description |
|--------|-------------|
| Color | Text color |
| Size | Font size (8-170 px) |
| Background Color | Widget background |

**Accepts:** All data types (Boolean, Integer, Float, String)

---

### Number Display

Shows numeric values with threshold-based color coding.

**Best for:** Sensor readings, counters, measurements

**Settings:**

| Option | Description |
|--------|-------------|
| Color | Default text color |
| Size | Font size |
| Min Value | Threshold for minimum color |
| Max Value | Threshold for maximum color |
| Min Color | Color when below minimum |
| Max Color | Color when above maximum |
| Decimals | Decimal places to display |

**Accepts:** Integer, Float

**Example:** Temperature display that turns blue below 10C and red above 30C.

---

### Gauge Widget

Displays values on a speedometer-style dial.

**Best for:** Percentages, levels, speeds, utilization metrics

**Settings:**

| Option | Description |
|--------|-------------|
| Min | Minimum gauge value |
| Max | Maximum gauge value |
| Segments | Number of colored sections |
| Needle Color | Pointer color |
| Start Color | Left-side gradient color |
| End Color | Right-side gradient color |
| Value Format | `.0f`, `.1f`, or `.2f` decimals |

**Accepts:** Integer, Float

---

### Bool State Widget

Shows boolean states with icons and labels.

**Best for:** On/Off states, alarms, door status, connection status

**Settings:**

| Option | Description |
|--------|-------------|
| True Icon | Icon when true (72 options) |
| False Icon | Icon when false |
| True Color | Color when true |
| False Color | Color when false |
| True Label | Text when true (e.g., "OPEN") |
| False Label | Text when false (e.g., "CLOSED") |
| Icon Size | Icon dimensions |
| Text Size | Label font size |

**Available Icons:** Power, toggle switches, checkboxes, locks, doors, network status, alerts, and more.

**Accepts:** Boolean

---

### Table Widget

Displays JSON arrays as scrollable tables.

**Best for:** Event logs, device lists, data records

**Settings:**

| Option | Description |
|--------|-------------|
| Headers | Column definitions (key-value pairs) |
| Auto-detect | Analyzes JSON to suggest headers |
| Show Export Button | Enable CSV export button (top-right corner) |
| Show Search Field | Enable search field to filter rows by any column value |

**Accepts:** String (JSON array)

**Example Input:**
```json
[
  {"time": "10:30", "event": "Motion", "zone": "Entry"},
  {"time": "10:45", "event": "Door Open", "zone": "Main"}
]
```

**Setup:**
1. Add the Table widget connected to a JSON output
2. Click **Auto-detect Headers** or manually define columns
3. Map JSON keys to display labels
4. Optionally enable **Show Export Button** for CSV downloads
5. Optionally enable **Show Search Field** to filter rows

**Features:**
- CSV export downloads all table data with configured column headers
- Search filters rows where any visible column contains the search term

---

### Chart Widget

Visualizes JSON data as line or bar charts.

**Best for:** Trends, historical data, time series, comparisons

**Settings:**

| Option | Description |
|--------|-------------|
| Chart Type | `bar` or `line` |
| X-Axis Mode | `index` (sequential) or `time` (datetime) |
| X-Axis Key | JSON property for time axis |
| Time Parse Mode | `iso`, `epoch_ms`, or `epoch_s` |
| Time Unit | second, minute, hour, day, week, month, quarter, year |
| Zoom/Pan | `none`, `x`, `y`, or `xy` |
| Grid Line Color | Chart grid color |

**Series Configuration:**

| Option | Description |
|--------|-------------|
| Data Key | JSON property to plot |
| Label | Series name in legend |
| Color | Line/bar color |
| Fill | Fill area under line |
| Tension | Line smoothness (0-1) |
| Point Radius | Data point size |
| Border Width | Line thickness |
| Stepped | Step-style line |

**Accepts:** String (JSON array)

**Example Input:**
```json
[
  {"datetime": "2025-01-01T10:00:00Z", "temperature": 22.5, "humidity": 45},
  {"datetime": "2025-01-01T11:00:00Z", "temperature": 23.1, "humidity": 43}
]
```

**Features:**
- Multiple data series on one chart
- Auto-detect series from JSON structure
- Zoom with mouse wheel or pinch
- Pan by dragging
- Reset button appears when zoomed

---

### AXIS Live Stream Widget

Displays live video from an Axis device using WebCodecs for low-latency H.264 decoding. Unlike WebRTC, this approach works reliably behind NAT and firewalls without requiring STUN/TURN servers.

**Best for:** Video monitoring, visual verification

**Settings:**

| Option | Description |
|--------|-------------|
| Device | Select from Device Management |
| Resolution | Stream resolution |

**Connection States:**

The widget shows connection progress:

- Connecting...
- Waiting for codec info...
- Initializing decoder...
- Streaming...

**Stream Statistics:**

Click the stats button when streaming to view real-time statistics:

| Stat | Description |
|------|-------------|
| Resolution | Current video resolution |
| Framerate | Frames per second |
| Bitrate | Stream bandwidth |
| Codec | Video codec (H.264) |
| Decode Queue | Frames waiting to be decoded |

**Note:** This widget does not require a Flow node connection. Errors are displayed directly in the stream area.

---

### Image Gallery Widget

Shows images from the Flow image cache.

**Best for:** Captured snapshots, event images, visual logs

**Settings:**

| Option | Description |
|--------|-------------|
| Grid Size | `small` (120px), `medium` (180px), `large` (240px), `xlarge` (320px) |
| Refresh Interval | Milliseconds between updates (default: 3000) |
| Show Metadata | Display file size overlay |

**Features:**
- Auto-refreshing grid of cached images
- Click to enlarge
- Download individual images
- Shows image metadata (size, content type)

**Note:** This widget does not require a Flow node connection.

---

### Button Control Widget

Interactive button to control Boolean Input or Button nodes from the dashboard.

**Best for:** Manual triggers, operator controls, on/off switches

**Settings:**

| Option | Description |
|--------|-------------|
| Mode | `toggle` (stays on/off) or `momentary` (on while pressed) |
| Active Color | Color when button is active |
| Inactive Color | Color when button is inactive |
| Active Icon | Icon when active |
| Inactive Icon | Icon when inactive |
| Active Label | Text when active (e.g., "ON") |
| Inactive Label | Text when inactive (e.g., "OFF") |
| Button Size | Button dimensions in pixels |
| Icon Size | Icon size in pixels |

**Accepts:** Boolean outputs from `node_button` or `node_bool_input` nodes only

**Note:** The button sends commands back to the Flow node via WebSocket.

---

### String Input Widget

Text input field to send string values to String Input nodes.

**Best for:** Text entry, search fields, configuration values

**Settings:**

| Option | Description |
|--------|-------------|
| Placeholder | Hint text when empty |
| Min Width | Minimum field width |
| Dense | Compact layout mode |
| Clearable | Show clear button |

**Accepts:** String outputs from `node_string_input` nodes only

**Note:** Press Enter or blur the field to send the value to the Flow.

---

### Static Text Widget

Displays static text without connecting to any node.

**Best for:** Labels, headers, instructions, section dividers

**Settings:**

| Option | Description |
|--------|-------------|
| Text Content | The text to display (supports multiple lines) |
| Text Size | Font size in pixels |
| Text Color | Text color (hex) |
| Text Align | Left, center, or right alignment |
| Bold | Enable bold text |
| Italic | Enable italic text |

**Note:** This widget does not require a Flow node connection.

---

### Media Image Widget

Displays a static image from the SD card media folder.

**Best for:** Logos, floor plans, reference images, static visuals

**Setup:**

1. Create a `media` folder on the SD card using the SD Card Explorer
2. Upload images (JPG, PNG, GIF, WebP) to the media folder
3. Add the Media Image widget to your dashboard
4. Select the image file from the dropdown

**Settings:**

| Option | Description |
|--------|-------------|
| Image File | Select from available images in media folder |
| Display Mode | `Contain` (fit inside), `Cover` (fill, may crop), `Fill` (stretch), `None` (original size) |

**Supported formats:** JPG, JPEG, PNG, GIF, WebP

**Note:** This widget does not require a Flow node connection.

---

### Media Video Widget

Displays a looping video from the SD card media folder.

**Best for:** Instructional videos, promotional content, ambient displays

**Setup:**

1. Create a `media` folder on the SD card using the SD Card Explorer
2. Upload videos (MP4, WebM) to the media folder
3. Add the Media Video widget to your dashboard
4. Select the video file from the dropdown

**Settings:**

| Option | Description |
|--------|-------------|
| Video File | Select from available videos in media folder |
| Display Mode | `Contain`, `Cover`, `Fill`, or `None` |
| Muted | Start video muted (default: enabled) |
| Loop | Loop video continuously (default: enabled) |
| Show Controls | Display video player controls (default: enabled) |
| Autoplay | Auto-start when dashboard goes live (default: enabled) |

**Supported formats:** MP4, WebM

**Note:** This widget does not require a Flow node connection. Autoplay requires muted to be enabled in most browsers.

---

### YouTube Widget

Embeds a YouTube video player.

**Best for:** Training videos, external content, video tutorials

**Settings:**

| Option | Description |
|--------|-------------|
| YouTube URL | Full YouTube URL or video ID (11 characters) |
| Autoplay | Auto-start video (requires muted) |
| Muted | Start video muted |
| Loop | Loop video continuously |

**Supported URL formats:**

- `https://www.youtube.com/watch?v=VIDEO_ID`
- `https://youtu.be/VIDEO_ID`
- `https://www.youtube.com/embed/VIDEO_ID`
- Direct video ID: `dQw4w9WgXcQ`

**Note:** This widget does not require a Flow node connection. Due to browser autoplay policies, autoplay typically requires the video to be muted.

---

### Clock Widget

Displays current time with configurable timezone and format.

**Best for:** Time displays, world clocks, shift schedules

**Settings:**

| Option | Description |
|--------|-------------|
| 24-hour format | Use 24-hour time (14:30) vs 12-hour (2:30 PM) |
| Show seconds | Include seconds in display |
| Show date | Display date below time |
| Timezone | Select timezone (local, UTC, or major cities) |
| Time Size | Font size for time display (12-200 px) |
| Time Color | Color of time text |
| Date Size | Font size for date display (8-72 px) |
| Date Color | Color of date text |

**Available Timezones:** Local, UTC, Europe (London, Paris, Berlin, Stockholm, Moscow), Americas (New York, Chicago, Denver, Los Angeles), Asia (Tokyo, Shanghai, Dubai, Singapore), Pacific (Sydney, Auckland)

**Note:** This widget does not require a Flow node connection.

---

### Markdown Widget

Displays formatted markdown text.

**Best for:** Instructions, notes, formatted documentation, announcements

**Settings:**

| Option | Description |
|--------|-------------|
| Markdown Content | Text with markdown formatting |
| Font Size | Base font size (8-48 px) |
| Text Color | Default text color |

**Supported Markdown:**

- `# Heading 1`, `## Heading 2`, `### Heading 3`
- `**bold**` and `*italic*`
- `~~strikethrough~~`
- `` `inline code` ``
- `[link text](url)`
- Lists with `-` or `*`
- Horizontal rules with `---`

**Note:** This widget does not require a Flow node connection.

---

### Weather Widget

Displays current weather conditions using Open-Meteo.

**Best for:** Environmental monitoring, outdoor activity planning, lobby displays

**Settings:**

| Option | Description |
|--------|-------------|
| Latitude | Location latitude (decimal degrees) |
| Longitude | Location longitude (decimal degrees) |
| Display Name | Optional location label shown in widget |
| Temperature Unit | Celsius or Fahrenheit (also affects wind speed) |
| Refresh Interval | How often to fetch new data (5, 15, 30 min, or 1 hour) |
| Show humidity and wind | Display additional weather details |
| Show location name | Display the location label |
| Icon Size | Weather icon size (24-128 px) |
| Temperature Size | Temperature text size (16-120 px) |
| Temperature Color | Color of temperature display |

**Displayed Information:**

- Current temperature with unit
- Weather condition icon (sunny, cloudy, rain, snow, fog, thunderstorm)
- Humidity percentage (optional)
- Wind speed in km/h or mph (optional)
- Location name (optional)
- Open-Meteo attribution

**Note:** This widget does not require a Flow node connection. Weather data is provided by Open-Meteo.

---

## Fullscreen Mode

Click the fullscreen button to enter a distraction-free viewing mode.

**In Fullscreen:**

- The entire dashboard view fills the screen (toolbar + widgets)
- Tabs remain visible for switching between dashboards
- Press **Escape** or click the fullscreen button again to exit

**Fullscreen in Live Mode:**

When fullscreen is active in Live Mode, edit controls are automatically hidden:

- Add dashboard (+) button
- Rename/delete tab buttons
- Add widget button

This provides a clean monitoring interface for operators.

---

## Widget Layout

### Positioning

In Edit Mode:
- **Drag** the handle icon (top-left) to move widgets
- **Resize** using the handle (bottom-right)
- Widgets snap to a grid for alignment
- Collision detection prevents overlapping

### Widget Appearance

Each widget has appearance settings:

| Option | Description |
|--------|-------------|
| Show Name | Display widget title |
| Widget Name | Custom title text |
| Background Color | Widget background |

Access via the **Appearance** tab in the widget properties panel.

### Deleting Widgets

Click the **delete icon** (top-right of widget) in Edit Mode.

---

## Dashboard Workflow

### For Administrators

1. Design your Flow with the nodes you want to monitor
2. Switch to Flow Dashboard
3. Enter Edit Mode
4. Add widgets for each value you want to display
5. Configure widget appearance and thresholds
6. Arrange widgets logically
7. Switch to Live Mode to verify
8. Set user permissions for operators

### For Operators

1. Open Flow Dashboard (loads in Live Mode)
2. Monitor real-time values
3. Use fullscreen mode for dedicated displays
4. Widgets update automatically as Flow data changes

---

## Permissions

Dashboard access is controlled by the permission system:

| Permission | Capability |
|------------|------------|
| `feature_flow_dashboard_read` | View dashboard in Live Mode |
| `feature_flow_dashboard_update` | Edit dashboard (add/remove/configure widgets) |

Operators typically have read-only access, while administrators can modify the dashboard layout.

---

## Best Practices

### Layout Tips

- Group related widgets together
- Use consistent sizing for similar data types
- Place critical information prominently
- Leave space for future additions

### Performance

- Limit the number of high-frequency updating widgets
- Use appropriate refresh intervals for Image Gallery
- Consider network bandwidth for live streams

### Data Visualization

- Match widget type to data type (gauges for ranges, tables for lists)
- Set meaningful thresholds for color-coded displays
- Use descriptive widget names
- Configure appropriate decimal places for numeric data

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Widget shows no data | Verify Flow is running and node is outputting data |
| Table shows "Invalid JSON" | Check that the connected output produces valid JSON array |
| Chart not updating | Verify JSON structure matches configured series keys |
| Live stream not connecting | Check device credentials in Device Management |
| Widget disappeared | Ensure you have edit permission; check if accidentally deleted |
