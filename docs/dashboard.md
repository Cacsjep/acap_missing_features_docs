# Dashboard

The Dashboard feature provides a customizable monitoring interface where you can visualize Flow node data using various widgets. Create real-time displays for operators without exposing the underlying Flow complexity.

---

## Overview

Dashboards display live data from your Flow nodes through configurable widgets. Each widget connects to a specific node input or output and updates in real-time.

**Key Features:**

- 8 widget types for different visualization needs
- Drag-and-drop widget placement
- Real-time data updates via WebSocket
- Edit and Live modes
- Permission-based access control

---

## Getting Started

### Opening the Dashboard

1. Navigate to **Flow Dashboard** from the main menu
2. The dashboard loads in **Live Mode** by default

### Switching Modes

| Mode | Description |
|------|-------------|
| **Live Mode** | View real-time data, no editing possible |
| **Edit Mode** | Add, configure, and arrange widgets |

Click **Switch to Edit Mode** to enable editing (requires `feature_flow_dashboard_update` permission).

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

Displays live video from an Axis camera.

**Best for:** Camera monitoring, visual verification

**Settings:**

| Option | Description |
|--------|-------------|
| Device | Select from Device Management |
| Resolution | Stream resolution |
| Width/Height | Display dimensions |

**Connection States:**
- Idle - Not connected
- Connecting - Establishing stream
- Connected - Live video
- Error - Connection failed (retries every 5 seconds)

**Note:** This widget does not require a Flow node connection.

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
