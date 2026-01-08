# Changelog

##### Legend
- **I:** Improvement
- **B:** Bugfix
- **F:** New Feature
- **C:** Change

##### V3.0.2 - 08.01.2026
- **F:** Flow: Multi-flow support with tabbed interface (run multiple flows simultaneously)
- **F:** Flow: Add manual Save button to toolbar (in addition to auto-save on run)
- **F:** Flow: Add Channel Publisher/Subscriber nodes for cross-flow communication
- **F:** Flow: Add Analytics Metadata node
- **F:** Flow: Add Tags input to Parking Monitor for categorizing entries
- **F:** Dashboard: Add Static Text widget for labels and headers (no node required)
- **F:** Dashboard: Add String Input widget for text entry controls
- **F:** Spot Color Detector feature for detecting colors in video regions
- **I:** Flow: Save confirmation toast notification
- **C:** Removed legacy Parking Times feature (use Parking Monitor node instead)
- **B:** Flow: Composites does not respect static input nodes.
- **B:** Flow: Composites parameter change not worked correctly.

##### V2.6.6 - 05.01.2026
- **F:** Spot Color Detection Added
- **I:** Flow: Add OPC UA Nodes
- **I:** Flow-Dashboard: Add Button Widget

##### V2.6.4 - 02.01.2026
- **B:** Flow: Map Set one cycle output fix
- **B:** Flow: Using AXIS Output Node with an IO Modul
- **F:** Flow: Add Collection Create Nodes
- **C:** Flow: License Plate Node emits now only for one cylce
- **C:** Flow: Remove raw json output from event subscribe node
- **I:** Flow: Rework Node Tags
- **I:** Flow: Add Space Key Bindings to open Node dialog


##### V2.6.3 - 31.12.2025
- **F:** Flow: Add Video Source Event types for subscribe
- **B:** Flow: Source and data keys fix
- **I:** Flow: Rework device management
- **I:** Flow: Add device discovery
- **I:** Flow: Add SQL Table and Read node

---

##### V2.5.0 - 25.12.2025
- **F:** Dashboard: Multi-dashboard support with browser-like tabs (add, rename, delete dashboards)
- **F:** Dashboard: New toolbar design with integrated tab management and action buttons
- **F:** Dashboard: Fullscreen mode includes entire view, hides edit controls in Live mode
- **F:** Dashboard: AXIS Live Stream widget now uses WebRTC for low-latency streaming
- **F:** Dashboard: Stream statistics overlay (resolution, framerate, bitrate, codec, packets lost, jitter)
- **F:** Dashboard: New dashboard feature for real-time data visualization with 8 widget types (Text, Number, Gauge, Bool State, Table, Chart, AXIS Live Stream, Image Gallery)
- **F:** Dashboard: Chart widget bar thickness configuration for better visualization control
- **F:** Flow: SQL Table node for visual table creation and data insertion without writing SQL
- **F:** Flow: SQL Table Read node for visual query building with relative time filters (last 15m, 1h, 7d, etc.)
- **F:** Flow: Modbus TCP Client node for industrial protocol connectivity
- **F:** Flow: Modbus Read/Write nodes for Coil, Int, Float, and String data types
- **F:** Flow: Collection nodes for List operations (Length, GetAt, SetAt, Push, Pop, InsertAt, RemoveAt, Contains, IndexOf, ForEach)
- **F:** Flow: Collection nodes for Map operations (Get, Set, Remove, ContainsKey, Keys, Values, Merge)
- **F:** Flow: Collection nodes for Set operations (Add, Remove, Contains)
- **F:** Flow: String manipulation nodes (Concat, Replace, Split, Join, Trim, Lower, Upper)
- **F:** Flow: Regex nodes (Match, Replace, FindAll)
- **F:** Flow: Encoding nodes (Base64 Encode/Decode)
- **F:** Flow: Image Cache nodes (Info, Delete) for in-memory image management
- **F:** Flow: JSON utility nodes (Parse, Get, Set, Remove, Validate, Placeholder JSON)
- **I:** Flow: Multi-database support for SQL nodes (SQLite, MySQL, PostgreSQL, SQL Server)
- **I:** Flow: SQL nodes now use local time instead of UTC for timestamp columns
- **I:** Documentation: Comprehensive updates for all new node categories and Dashboard feature
- **I:** Documentation: SQL Table and SQL Table Read nodes with visual query examples
- **I:** Documentation: Full SQLite tutorial with database creation, queries, and practical examples
- **I:** Documentation: Input nodes, Seven Segment display, and detailed HTTP Request configuration

---

##### V2.4.5 - 22.12.2025
- **I:** Flow: Add WebRtc Node
- **I:** Flow: Add Rtmp Stream Node
- **B:** Flow: Fix fast Node drag not work from node selector
- **B:** Fix Redirect on same page not work correctly

---

##### V2.4.1 - 21.12.2025
- **F:** Flow: Added the node test harness that runs Axis, HTTP/Webhook, SD card, TCP, and SQL nodes locally, plus the TCP Server/Sender nodes, Axis SD-Card helpers, and SQL CRUD nodes so developers can verify behavior before runtime.
- **I:** Switch from reverse proxy mode into full reachable webserver mode, what includes seperate user and role/permission system to have control about non admin user tasks in webui.
- **C:** Removed features: Metadata Dispatch and Occupancy Counting no longer ship with this release.

---

##### V2.2.6 - 16.12.2025
- **F:** Flow: Added the Axis Parameter node so flows can read any AXParameter key as a string and publish both the value and any ParamHandler error.

---

##### V2.2.5 - 15.12.2025
- **F:** Flow: Added the TCP Server node for raw TCP listeners (`New Message` pulse, message/error outputs, per-second rate limiting), plus the TCP Message Sender that triggers on a rising send input and reports success/error.
- **F:** Flow: Added the Webhook Listener node (HTTP/HTTPS with Basic/Digest auth, JSON responses, configurable query-param outputs, and a one-cycle `New Request` pulse) and documented the JSON response contract in the Flow guide.
---

##### V2.2.3 - 14.12.2025
- **B:** Flow: Axis RTSP stream node validation now checks correctly to avoid rejecting valid configurations.
- **I:** Flow: Axis RTSP stream node now validates and displays custom width/height settings, offers a custom-resolution picker, resyncs the stream when the provider client is removed, and logs frame-provider tear-downs to surface streaming issues more clearly.
- **I:** Flow: Realigned the image-routing helpers with the cached JPEG store so the HTTP fetch routes and the embedded JPEG viewer stay in sync with the shared memory layer.
---

##### V2.2.0 - 12.12.2025
- **F:** Flow: Added the Scene Analysis Triggers feature (`metadata_trigger`) with backend helpers, live `VdoLiveStream`, SVG overlays, editable area/line triggers, bounding boxes, direction markers, filter controls, WebSocket metadata, and event hooks that persist the trigger definitions.
- **F:** Flow: Introduced the `JsonList` node plus column-management UI for splitting array payloads into per-column outputs.
- **I:** Flow: Refactored Vaxtor Cloud ANPR outputs into structured metadata (plate, vehicle, confidence, status) and emit an `Image ID` so downstream viewers can load cached snapshots without moving the JPEG bytes over WebSocket.
- **I:** Flow: Polished the metadata-triggers visualization helpers with cross-line tracking, ROI conversions, and smoother highlight/update logic for the overlay controls.

---

##### V2.1.5 - 09.12.2025
- **I:** Flow: Add Composite Nodes
- **I:** Flow: Add Teleport Nodes
- **I:** Flow: UI Improvement

---

##### V2.1.2 - 06.12.2025
- **F:** Flow: Added edge value muxer
- **B:** Flow: Fix invalid port usage of rtsp port

---

