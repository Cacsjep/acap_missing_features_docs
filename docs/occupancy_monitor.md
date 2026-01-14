# Occupancy Monitor

The Occupancy Monitor feature provides multi-zone occupancy counting with configurable thresholds and real-time monitoring. It aggregates counting events from multiple cameras to track occupancy levels across different areas.

---

## Overview

The Occupancy Monitor allows you to:

- Define multiple occupancy zones
- Aggregate counts from multiple cameras per zone
- Set warning and maximum thresholds
- Monitor occupancy in real-time via WebSocket
- Trigger events when thresholds are exceeded
- Manually adjust counts via the UI or API

---

## Configuration

### Creating Zones

1. Navigate to the **Occupancy Monitor** feature
2. Click the **+** icon next to "Occupancy Zones" to add a new zone
3. Configure the zone settings

### Zone Settings

| Setting | Description |
|---------|-------------|
| **Zone Name** | A descriptive name for the zone (e.g., "Lobby", "Parking Level 1") |
| **Use Thresholds** | Enable warning and maximum occupancy levels |
| **Warning Level** | Occupancy count that triggers a warning state |
| **Maximum Level** | Maximum allowed occupancy count |

### Camera Configuration

Each zone can have multiple cameras configured to count entries and exits:

| Setting | Description |
|---------|-------------|
| **Camera Name** | Descriptive name for the camera |
| **IP Address** | IP address of the Axis camera |
| **Username** | Camera login username |
| **Password** | Camera login password |
| **Use HTTPS** | Enable secure connection to the camera |
| **Role** | Camera role: Entry, Exit, or Entry and Exit |
| **Is Internal** | Mark as internal camera (uses local device events) |

### Camera Roles

| Role | Description |
|------|-------------|
| **Entry** | Camera counts people/vehicles entering the zone |
| **Exit** | Camera counts people/vehicles exiting the zone |
| **Entry and Exit** | Camera counts both directions |

---

## Event Configuration

Configure which AXIS events trigger count changes:

| Setting | Description |
|---------|-------------|
| **Topic Path** | The AXIS event topic to subscribe to |
| **Event Name** | Name identifier for the event |
| **Source Fields** | Event source fields to match |
| **Data Fields** | Event data fields to extract |
| **Stateful** | Whether the event maintains state |

### Delete Event

Optionally configure an event that resets or clears occupancy counts:

| Setting | Description |
|---------|-------------|
| **Use Delete Event** | Enable automatic count reset via event |
| **Delete Event Config** | Configure the event that triggers count reset |

---

## Real-Time Monitoring

The **Occupancy Monitor** panel displays real-time occupancy data for all configured zones:

- **Current Count**: The current occupancy number
- **Status Indicators**: Color-coded based on thresholds
    - Normal (default): Below warning level
    - Warning (orange): At or above warning level
    - Maximum (red): At or above maximum level
- **Manual Adjustment**: Use +/- buttons to manually adjust counts

---

## Manual Count Adjustment

You can manually adjust occupancy counts:

1. In the Monitor panel, locate the zone
2. Click the **-** button to decrease count
3. Click the **+** button to increase count

This is useful for:

- Correcting counting errors
- Initial calibration
- Manual overrides

---

## Integration with Flow

The Occupancy Monitor integrates with Flow:

- Use occupancy data as input to Flow logic
- Trigger actions when thresholds are exceeded
- Send notifications based on occupancy levels

