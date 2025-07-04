# Features

Below is an overview of the features provided by Missing Feature ACAP. 

---

#### Occupancy Counting

Occupancy counting using multiple cameras.

#### Event Camera Bridge

Register events from other AXIS cameras into the camera rules system to enable cross-camera rules.

#### Event Delay Timer

Introduces a configurable delay between the original event trigger and a subsequent action. This allows you to sequence events and automate workflows that depend on precise timing.

#### Event Sunrise and Sunset

Retrieves daily sunrise and sunset data and triggers corresponding events. Perfect for adjusting camera settings, turning on lights, or starting/stopping recordings according to natural light levels.

#### Event Timed Sequence

Triggers only if a second event follows the first within a defined time window. This feature helps in creating complex event chains, ensuring that actions are executed only under specific, timely conditions.

#### Image to SD Card

Automatically saves an image whenever a specified event occurs (for example, during motion detection). You can configure both the retention period and the maximum storage capacity (in GB) to suit your requirements. With built-in video generation, snapshots can be transformed into a timelapse sequence—ideal for long-term observation and quick playback of key events.

#### Image to FTP

Automatically transfer and delete recorded jpeg images whenever a specified event occurs.
The problem with actual camera FTP sending, is that you cant have a pre buffer over 99 seconds. With this feature there is no such limit.

#### Metadata Dispatch

Unified metadata and metrics dispatcher for databases, SD-Card, MQTT, OPCUA or HTTPS servers.

#### Notifications 

Send SMS to Teltonika devices, or sms/phone calls via [seven.io](https://seven.io/)

#### Parking Times

Logs the in and out timestamps of vehicles identified by the AXIS License Plate Verifier. This feature maintains a detailed record of parking durations and triggers an event if a vehicle exceeds the designated time. It enables timely notifications, enforcement actions, or other workflow automations.


