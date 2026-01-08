# Spot Color Detector

The Spot Color Detector analyzes video frames to detect specific colors within defined circular regions. When a color changes, it triggers AXIS events that can be used in rules or captured by Flow nodes.

---

## Overview

Define circular "spots" on the video feed and configure a color palette. The system continuously analyzes each spot and detects which palette color is currently visible. When the color changes, an AXIS event is fired.

**Use Cases:**

- Traffic light detection (red/yellow/green states)
- Status indicator monitoring (LED colors on equipment)
- Industrial process monitoring (color-coded stages)
- Presence detection based on color changes

---

## Getting Started

1. Navigate to **Spot Color Detector** from the feature menu
2. The live video preview loads automatically
3. Click **Add Circle** to create a detection region
4. Position and resize the circle over the target area
5. Configure the color palette with expected colors
6. Start the feature to begin detection

---

## Configuration

### Video Settings

| Setting | Description |
|---------|-------------|
| Video Channel | Select the video channel (default: 1) |
| Resolution | Analysis resolution (lower = faster processing) |
| Hysteresis Threshold | Color distance threshold to prevent flickering (10-100) |

### Circle Triggers

Each circle defines a detection region:

| Property | Description |
|----------|-------------|
| Name | Unique identifier for the circle (used in events) |
| Position | X/Y center coordinates (drag to move) |
| Radius | Detection area size (drag corner to resize) |

**Adding Circles:**

1. Click **Add Circle** button
2. Enter a name for the circle (e.g., "traffic_light_1")
3. Drag the circle to position it over the target
4. Resize using the corner handle

**Editing Circles:**

- Click the pencil icon to rename
- Click the trash icon to delete
- Drag to reposition
- Drag the resize handle to change size

### Color Palette

The palette defines which colors the system recognizes:

**Default Colors:**
- Basic: Red, Green, Blue, Yellow, Orange, Purple, Cyan, Magenta, White, Black, Gray
- Traffic: Traffic Red, Traffic Yellow, Traffic Green, Off/Dark

**Managing Colors:**

| Action | How To |
|--------|--------|
| Add Color | Click "Add Color", pick a color and enter a name |
| Add Detected | Click "Add to Palette" on a circle's detected color |
| Remove Color | Click the X on a color chip |
| Reset Palette | Click "Reset" to restore defaults |

---

## How It Works

### Color Detection

1. The system captures video frames at the configured resolution
2. For each circle, pixels within the boundary are sampled
3. The dominant color (average RGB) is calculated
4. This color is matched to the nearest palette color
5. If the color changed beyond the hysteresis threshold, an event fires

### Hysteresis

The hysteresis threshold prevents rapid flickering between similar colors:

- **Low value (10-20):** More sensitive, may flicker in variable lighting
- **High value (50-100):** More stable, requires larger color differences
- **Default (30):** Balanced for most scenarios

The threshold represents Euclidean distance in RGB color space.

### Processing Time

The UI displays processing time per frame:

- **Green:** Under budget, system running smoothly
- **Red:** Over budget, consider reducing resolution or circle count

---

## Events

Each circle generates an AXIS event when its color changes:

**Event Name:** `{circle_name}_state`

**Event Data:**

| Field | Description |
|-------|-------------|
| circle_name | Name of the circle |
| color_name | Matched palette color name |
| confidence | Match confidence (0-1) |

### Using Events in AXIS Rules

While the Spot Color Detector is running, events appear in the AXIS event system:

1. Open the device's web interface
2. Go to **System > Events**
3. Find events under the Missing Features ACAP
4. Create rules triggered by color state changes

### Using Events in Flow

Use the **Axis Metadata Event (Subscribe)** node to receive color change events:

1. Add the subscribe node to your flow
2. Select the Spot Color Detector event
3. Wire the outputs to your logic

---

## Live Preview

The preview shows:

- Real-time video feed via WebRTC
- Circle overlays with current state
- For each circle:
  - Circle name
  - Detected color (actual RGB from frame)
  - Matched color (nearest palette color)
  - Edit/Delete controls

### Controls

| Button | Action |
|--------|--------|
| Add Circle | Create new detection region |
| Show/Hide | Toggle circle overlay visibility |
| Fullscreen | Expand preview to full screen |

---

## Best Practices

### Circle Placement

- Position circles directly over the color area
- Avoid edges where background colors might interfere
- Use smaller circles for precise targets (LEDs, indicators)
- Use larger circles for areas with color variation

### Palette Configuration

- Add only colors you expect to detect
- Use specific shades (e.g., "Traffic Red" vs generic "Red")
- Test detection and adjust palette colors if needed
- Add the "Off/Dark" color for inactive states

### Performance

- Use lower resolution for faster processing
- Limit the number of circles (each adds processing time)
- Monitor the processing time indicator
- Increase hysteresis if colors flicker

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Color not detected | Ensure the palette includes a similar color |
| Flickering between colors | Increase the hysteresis threshold |
| Wrong color matched | Add a more specific color to the palette |
| High processing time | Reduce resolution or circle count |
| Events not firing | Check that the feature is started |
