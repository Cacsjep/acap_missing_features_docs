# Parking Management

The Parking Management feature provides comprehensive parking lot management with license plate recognition, zone-based tracking, access control automation, and detailed statistics.

---

## Overview

Parking Management offers:

- **Multi-zone support** with individual capacity limits and utilization tracking
- **Entry/Exit camera integration** with automatic plate matching
- **Snapshot capture** on entry and exit for evidence
- **Tag-based access rules** with configurable max parking times
- **IO Actions** for barrier control and signage
- **Real-time monitoring** via WebSocket with live zone occupancy
- **History & Statistics** with filtering, export, and analytics

---

## Tabs

The Parking Management interface is organized into tabs:

| Tab | Description |
|-----|-------------|
| **Live** | Real-time zone occupancy, camera status, plate event log, and manual IO actions |
| **History** | Searchable parking entry history with table/grid views and CSV export |
| **Zones** | Configure parking zones with entry/exit cameras |
| **Access & Events** | IO actions, event rules, and email notifications for overparking |
| **Plate List** | Embedded License Plate List for managing plates and tags |
| **General** | Global settings like stale timeout, history retention, and snapshot cleanup |

!!! tip "Integrated Plate Management"
    The Plate List tab provides direct access to the License Plate List feature without leaving Parking Management. Changes to plates and tags are automatically available in Zones and Access Control when switching tabs.

---

## Zones Configuration

### Creating Zones

Each parking zone represents a physical area (parking level, section, or lot):

| Setting | Description |
|---------|-------------|
| **Zone Name** | Descriptive name (e.g., "Level 1", "Visitor Parking") |
| **Capacity** | Maximum number of vehicles (0 = unlimited) |
| **Default Stale Timeout** | Minutes before parked entries are marked as stale (overrides global setting) |

!!! note "Zone Full Behavior"
    When a zone reaches capacity, new entries are still recorded for tracking purposes and a "Zone Full" event is fired. Use IO Rules to trigger barriers or signage when capacity is reached.

### Entry/Exit Cameras

Each zone can have multiple cameras configured as entry, exit, or both:

| Setting | Description |
|---------|-------------|
| **Camera** | Select from registered Flow Devices |
| **Role** | Entry, Exit, or Entry & Exit |
| **Capture Snapshot** | Store entry/exit images on SD card |

!!! tip "Managing Devices"
    Use the **Devices** button in the toolbar (visible on Zones and Access Control tabs) to add or configure cameras and IO devices.

#### Camera Roles

| Role | Behavior |
|------|----------|
| **Entry** | Creates new parking entry when plate detected |
| **Exit** | Matches plate to existing entry and records exit time |
| **Entry & Exit** | Handles both directions (single-point access) |

---

## Tag-Based Rules

Tags from the License Plate List determine parking rules:

| Setting | Description |
|---------|-------------|
| **Tag** | Tag name from License Plate List (e.g., "staff", "visitor", "vip") |
| **Max Parking Minutes** | Maximum allowed parking time (0 = unlimited) |
| **Zones** | Which zones this rule applies to (empty = all zones) |

When a plate is detected, the system:

1. Looks up the plate in the License Plate List
2. Validates the plate's date range (From/To dates if set)
3. Finds matching tags
4. Applies the most restrictive max parking time
5. Marks entry as overtime when exceeded

!!! note "Date Validation"
    Plates with a **From Date** that hasn't been reached yet, or a **To Date** that has passed, will be denied access even if they have valid tags.

---

## Access & Events

The Access & Events tab contains IO actions, event rules, and email notifications for overparking alerts.

### IO Actions

Manual actions that can be triggered from the Live view or camera preview:

| Setting | Description |
|---------|-------------|
| **Name** | Display name for the button |
| **Device** | Target device with IO ports |
| **Port** | IO port number to control |
| **Action** | Pulse, Activate, or Deactivate |
| **Duration** | Pulse duration in milliseconds (for Pulse action) |
| **Show on cameras** | Select which cameras should display this action button in their live preview |

!!! tip "Camera Preview Actions"
    When you configure "Show on cameras" for an IO action, the action button will appear below the video stream when viewing that camera's live preview. This is useful for triggering barriers or gates while watching the camera feed.

### IO Rules

Automated actions triggered by parking events:

| Setting | Description |
|---------|-------------|
| **Name** | Rule identifier |
| **Event** | Entry, Exit, Unauthorized, Overtime, or Zone Full |
| **Zones** | Which zones trigger this rule (empty = all) |
| **Tags** | Which tags trigger this rule (empty = all) |
| **Device** | Target device |
| **Port** | IO port number |
| **Action** | Pulse, Activate, or Deactivate |

#### Event Types

| Event | Description |
|-------|-------------|
| **Entry** | Vehicle entered a zone |
| **Exit** | Vehicle exited a zone |
| **Unauthorized** | Unknown plate or plate without allowed tags |
| **Overtime** | Vehicle exceeded max parking time |
| **Zone Full** | Zone reached its capacity limit |

#### Example Rules

| Use Case | Event | Condition | Action |
|----------|-------|-----------|--------|
| Open barrier for staff | Entry | Tag = "staff" | Pulse port 1 |
| Open barrier for visitors | Entry | Tag = "visitor" | Pulse port 1 |
| Open exit barrier | Exit | Any | Pulse port 2 |

### Email Notifications

Receive email alerts when vehicles exceed their maximum parking time.

| Setting | Description |
|---------|-------------|
| **Enable** | Toggle email notifications on/off |
| **SMTP Host** | Email server address (e.g., smtp.gmail.com) |
| **Port** | SMTP port (587 for TLS, 465 for SSL) |
| **Username** | SMTP authentication username |
| **Password** | SMTP authentication password |
| **Use TLS/SSL** | Enable encrypted connection |
| **From Address** | Sender email address |
| **Recipients** | Comma-separated list of recipient emails |
| **Zones** | Select specific zones or leave empty for all zones |
| **Attach Entry Snapshot** | Include the entry photo in the email |

!!! tip "Gmail Setup"
    For Gmail, use `smtp.gmail.com` with port 587 and TLS enabled. You'll need to create an App Password in your Google Account settings (Security → 2-Step Verification → App passwords).

!!! note "When Emails Are Sent"
    An email is sent when a vehicle's parking duration exceeds its maximum allowed time. The system checks every minute. Each vehicle triggers only one email per parking session.

#### Test Email

Use the **Test** button to verify your SMTP configuration. The test sends a sample email to all configured recipients.

---

## Live View

The Live tab displays real-time parking information with statistics and monitoring tools.

### Top Bar Statistics

The top bar shows key metrics at a glance:

| Stat | Description |
|------|-------------|
| **Parked** | Total vehicles currently parked |
| **Avg Duration** | Average parking time of current vehicles |
| **Zone Capacity** | Occupancy per zone (e.g., "Level 1: 5/20") |
| **Overtime** | Count of vehicles exceeding max parking time |
| **Utilization** | Overall parking lot utilization percentage |

### Parked Vehicles

Shows all currently parked vehicles with:

- Header row with column labels
- Time parked (duration)
- Vehicle name (from License Plate List)
- Zone and camera
- License plate number
- Tag assignment
- Parking progress bar (visual indicator of parking duration)

### Zone Cards

Each zone shows:

- Current occupancy vs. capacity
- Utilization percentage bar
- List of currently parked vehicles with duration

### Camera Status

Shows health of all configured cameras:

- **Green dot**: Camera online and connected
- **Red dot**: Camera offline or connection lost
- Last detected plate
- Last activity timestamp

!!! note "Multi-Zone Cameras"
    A single camera can serve multiple zones (e.g., nested parking areas). The camera status shows all zones it serves, and events are processed for each zone independently.

!!! note "Real-time Status"
    Camera status updates automatically when a camera disconnects or reconnects - no page refresh needed.

### Plate Event Log

Real-time feed of plate detections with detailed event information:

| Column | Description |
|--------|-------------|
| **ID** | Unique event identifier |
| **Time** | Detection timestamp |
| **State** | Event state (Entry, Exit, Entry Denied, etc.) |
| **Plate** | License plate number |
| **Name** | Name from License Plate List |
| **Zone** | Parking zone |
| **Camera** | Camera that detected the plate |
| **Type** | Detection type |
| **Color** | Vehicle color (if detected) |
| **Size** | Image size in pixels |
| **Act** | Action status |
| **Img** | Snapshot thumbnail (click to view) |

#### Event States

| State | Color | Description |
|-------|-------|-------------|
| **Entry** | Green | Vehicle entered the zone |
| **Exit** | Blue | Vehicle exited the zone |
| **Entry Denied** | Red | Entry denied (unauthorized or zone full) |
| **Tag Not Allowed** | Orange | Plate has tag but not allowed in this zone |
| **Exit (No Entry)** | Cyan | Exit detected without matching entry |
| **Add to Plate List** | Purple | Plate added to License Plate List |

### Manual Actions

Execute IO Actions directly from the interface (requires `parking_execute_action` permission).

---

## History View

### Table View

Sortable data table with columns:

| Column | Description |
|--------|-------------|
| **Plate** | License plate number |
| **Name** | Name from License Plate List (if matched) |
| **Zone** | Parking zone |
| **Entry Time** | When vehicle entered |
| **Exit Time** | When vehicle exited |
| **Duration** | Total parking time |
| **Status** | Parked, Exited, or Stale |
| **Tag** | Matched tag from plate list |
| **Overtime** | Warning if exceeded max time |
| **Snapshots** | Click to view entry/exit images |

### Grid View

Visual card layout showing entry snapshots with plate overlay.

### Filters

| Filter | Description |
|--------|-------------|
| **Plate** | Search by plate number |
| **Status** | Parked, Exited, or Stale |
| **Zone** | Filter by zone |
| **Tag** | Filter by matched tag |
| **From/To Date** | Date range filter |

### Export

Download filtered entries as CSV file.

### Entry Actions

Right-click or use action menu on entries:

| Action | Description |
|--------|-------------|
| **Edit** | Correct plate number |
| **Extend** | Add extra parking time |
| **Add to List** | Add plate to License Plate List with tags |
| **Delete** | Remove entry |

---

## General Settings

| Setting | Description |
|---------|-------------|
| **Default Stale Timeout** | Minutes before entries without exit are marked stale (default: 1440 = 24h) |
| **History Retention Days** | How long to keep history entries (default: 7 days) |
| **Snapshot Max GB** | Maximum SD card storage for snapshots |
| **Snapshot Cleanup Days** | Delete snapshots older than this |

### Cleanup Actions

- **Delete All Parked**: Remove all currently parked entries
- **Delete All History**: Clear all historical entries

---

## Events

Parking Management generates AXIS Metadata events:

| Event | Description |
|-------|-------------|
| **Entry** | Vehicle entered a zone |
| **Exit** | Vehicle exited a zone |
| **Unauthorized** | Unknown plate or plate without allowed tags attempted entry |
| **Overtime** | Vehicle exceeded max parking time |
| **Zone Full** | Zone reached capacity limit |

Events include metadata: plate, zone, tag, duration, camera name.

!!! tip "Event Automation"
    Use IO Rules in the Access Control tab to trigger actions (open barriers, activate lights, sound alarms) when these events occur.

---

## Comparison: Parking Management vs. Parking Monitor Node

| Feature | Parking Management | Parking Monitor Node |
|---------|-------------------|---------------------|
| **Type** | Standalone feature | Flow node |
| **Zones** | Multi-zone with capacity | No zone concept |
| **Cameras** | Built-in camera management | Manual event wiring |
| **Snapshots** | Automatic capture | Manual with flow |
| **Access Control** | Built-in IO actions/rules | Manual with flow |
| **Statistics** | Built-in dashboard | Manual with flow |
| **History UI** | Table/Grid with filters | Basic table |
| **Best For** | Complete parking solution | Custom integrations |

