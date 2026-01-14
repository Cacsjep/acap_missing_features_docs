# Sunrise & Sunset Events

The Sunrise & Sunset Events feature creates AXIS platform events based on actual sunrise and sunset times for your location. This enables time-based automation that adapts to seasonal daylight changes.

---

## Overview

This feature:

- Fetches accurate sunrise/sunset times based on GPS coordinates
- Creates AXIS platform events at sunrise and sunset
- Automatically updates daily for seasonal changes
- Provides a "daylight" state for use in AXIS rules
- Uses the [SunriseSunset.io](https://sunrisesunset.io/) API

---

## Configuration

### Setting Your Location

1. Navigate to the **Event Sunset and Sunrise** feature
2. Click on the interactive map to set your location
3. The latitude and longitude will update automatically
4. Click **Save** to apply the settings

!!! tip "Click to Set Location"
    Simply click anywhere on the map to set your coordinates. The marker will move to the clicked position.

### Map Controls

- **Zoom**: Use scroll wheel or +/- buttons
- **Pan**: Click and drag the map
- **Set Location**: Click to place the marker

---

## Generated Events

The feature registers the following AXIS platform event:

### Sun Events

| Property | Type | Description |
|----------|------|-------------|
| **daylight** | Boolean | `true` during daylight hours, `false` at night |

This event can be used in:

- AXIS Camera Station rules
- AXIS device action rules
- Third-party VMS systems
- Flow automation

---

## How It Works

1. **Daily Schedule**: At 3:00 AM local time, the feature fetches sunrise/sunset times for the current day
2. **Sunrise Event**: At the calculated sunrise time, the `daylight` property changes to `true`
3. **Sunset Event**: At the calculated sunset time, the `daylight` property changes to `false`
4. **Initial State**: On startup, the current daylight state is determined based on the current time

---

## Use Cases

### Automatic Day/Night Switching

Use the daylight event to:

- Switch between day and night video profiles
- Enable/disable certain analytics during darkness
- Adjust PTZ presets based on time of day

### Lighting Control

Trigger external lighting systems:

- Turn on lights at sunset
- Turn off lights at sunrise
- Integrate with building management systems

### Recording Schedules

Modify recording behavior:

- Higher quality recording during daylight
- Motion-only recording at night
- Different retention policies by time of day

---

## Integration with AXIS Rules

Create rules in your AXIS device using the Sun Events:

1. Go to **System > Events** in the camera web interface
2. Create a new rule
3. Select **Application > Sun Events** as the condition
4. Configure the action (e.g., switch video profile)

### Example Rules

**Day Profile at Sunrise:**
```
Condition: Application > Sun Events > daylight = true
Action: Set video profile to "Day"
```

**Night Profile at Sunset:**
```
Condition: Application > Sun Events > daylight = false
Action: Set video profile to "Night"
```

---

## Integration with Flow

Use the sunrise/sunset events in Flow:

- Subscribe to the Sun Events topic
- Use the daylight state as a condition
- Trigger different flows for day vs night operation

---

## API Endpoint

Get current sunrise/sunset information:

```
GET /api/sunsetinfo
```

Response:
```json
{
  "timesavailable": true,
  "sunrise": "2024-01-15 07:45:32",
  "sunset": "2024-01-15 16:38:15",
  "sunriseAt": "scheduled",
  "sunsetAt": "scheduled"
}
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Events not firing | Verify location is set and feature is enabled |
| Wrong times | Check that coordinates are correct for your location |
| Map not loading | Ensure network connectivity to tile servers |
| "Already passed" status | Normal if current time is after sunrise/sunset; will update next day |

!!! note "Internet Required"
    This feature requires internet access to fetch sunrise/sunset times from the SunriseSunset.io API. Ensure the device can reach external HTTPS endpoints.
