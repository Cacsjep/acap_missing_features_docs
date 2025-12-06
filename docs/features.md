# Features

Below is an overview of the features provided by Missing Feature ACAP. 

---

#### Flow

Design automation visually by dragging nodes onto a canvas and wiring them together. Includes Axis device nodes, HTTP/JSON utilities, Lua scripting, device management (per-device credentials, snapshots/resolutions reused across nodes), and type-safe wiring with per-node help.

#### Image to SD Card

Automatically saves an image whenever a specified event occurs (for example, during motion detection). You can configure both the retention period and the maximum storage capacity (in GB) to suit your requirements. With built-in video generation, snapshots can be transformed into a timelapse sequence—ideal for long-term observation and quick playback of key events.

#### Metadata Dispatch

Unified metadata and metrics dispatcher for databases, SD-Card, MQTT, OPCUA or HTTPS servers.

#### Notifications 

Send SMS to Teltonika devices, or sms/phone calls via [seven.io](https://seven.io/)

#### Occupancy Counting

Occupancy counting using multiple cameras.

#### Parking Times

Logs the in and out timestamps of vehicles identified by the AXIS License Plate Verifier. This feature maintains a detailed record of parking durations and triggers an event if a vehicle exceeds the designated time. It enables timely notifications, enforcement actions, or other workflow automations.

#### License Plate List

Whitelist plates with validity windows, tags, and optional fuzzy matching. Cached list feeds the Flow License Plates node and can auto-clean expired entries. See `license_plate_list.md` for setup.


