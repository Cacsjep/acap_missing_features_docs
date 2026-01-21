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
| **Access Control** | Set up IO actions and automated access rules |
| **General** | Global settings like stale timeout, history retention, and snapshot cleanup |

---

## Zones Configuration

### Creating Zones

Each parking zone represents a physical area (parking level, section, or lot):

| Setting | Description |
|---------|-------------|
| **Zone Name** | Descriptive name (e.g., "Level 1", "Visitor Parking") |
| **Capacity** | Maximum number of vehicles (0 = unlimited) |
| **Default Stale Timeout** | Minutes before parked entries are marked as stale (overrides global setting) |

### Entry/Exit Cameras

Each zone can have multiple cameras configured as entry, exit, or both:

| Setting | Description |
|---------|-------------|
| **Camera** | Select from registered Flow Devices |
| **Role** | Entry, Exit, or Entry & Exit |
| **Capture Snapshot** | Store entry/exit images on SD card |

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
2. Finds matching tags
3. Applies the most restrictive max parking time
4. Marks entry as overtime when exceeded

---

## Access Control

### IO Actions

Manual actions that can be triggered from the Live view:

| Setting | Description |
|---------|-------------|
| **Name** | Display name for the button |
| **Device** | Target device with IO ports |
| **Port** | IO port number to control |
| **Action** | Pulse, Activate, or Deactivate |
| **Duration** | Pulse duration in milliseconds (for Pulse action) |

### IO Rules

Automated actions triggered by parking events:

| Setting | Description |
|---------|-------------|
| **Name** | Rule identifier |
| **Event** | Entry, Exit, or Overtime |
| **Zones** | Which zones trigger this rule (empty = all) |
| **Tags** | Which tags trigger this rule (empty = all) |
| **Device** | Target device |
| **Port** | IO port number |
| **Action** | Pulse, Activate, or Deactivate |

#### Example Rules

| Use Case | Event | Condition | Action |
|----------|-------|-----------|--------|
| Open barrier for staff | Entry | Tag = "staff" | Pulse port 1 |
| Red light on full | Entry | Zone at capacity | Activate port 2 |
| Alert on overtime | Overtime | Any | Pulse alarm port |

---

## Live View

The Live tab displays:

### Zone Cards

Each zone shows:

- Current occupancy vs. capacity
- Utilization percentage bar
- List of currently parked vehicles with duration

### Camera Status

Shows health of all configured cameras:

- Online/Offline indicator
- Last detected plate
- Last activity timestamp

### Plate Event Log

Real-time feed of plate detections:

- Plate number
- Camera name
- Timestamp
- Zone assignment
- Match status (known/unknown)

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
| **Overtime** | Vehicle exceeded max parking time |

Events include metadata: plate, zone, tag, duration, camera name.

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

