# Changelog

##### Legend
- **I:** Improvement
- **B:** Bugfix
- **F:** New Feature
- **C:** Change

---

##### V2.5.0 - 25.12.2025
- **F:** Dashboard: New dashboard feature for real-time data visualization with 8 widget types (Text, Number, Gauge, Bool State, Table, Chart, AXIS Live Stream, Image Gallery)
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
- **I:** Documentation: Comprehensive updates for all new node categories and Dashboard feature
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

##### V2.1.0 - 04.12.2025
- **F:** Flow: Added Vaxtor Cloud ANPR node (Axis snapshot upload with OAuth2 client credentials, token caching, plate & vehicle outputs, success/error status).
- **F:** Flow: Added JSON-to-Node and JSON Array Selector nodes to split API responses into per-key outputs or indexed items.
- **F:** Flow: Added License Plate List node for whitelists with validity windows and per-tag outputs.
- **I:** Flow: Device management integrated into Flow (shared device list, non-video flag, resolution-aware snapshots reused by nodes like HTTP image uploads and Vaxtor).
- **I:** Flow: Connection safety refinements and help coverage for the new nodes.

---

##### V2.0.39 – 28.10.2025
- **I:** Flow: Prevent multiple connection to inputs
- **I:** Flow: Prevent connetion with diffrent data types
- **B:** Flow: Connection saving failed when validation multiple connections
- **I:** Flow: Add safe mode to turn on or off invalid connection message 
- **I:** Flow: Add Lua Node
- **I:** Flow: Add Lua json methods
- **I:** Flow: Add Lua time methods
- **B:** Flow: Fix invalid shape on inputs

---

##### V2.0.31 – 27.10.2025
- **B:** Flow: Osci was not correclty renderd when flow was alread running
- **I:** Flow: Node Help Dialog Added
- **I:** Flow: Improve UI, Colors, Node Action Bar
- **I:** Flow: Bend points now added on mouse hold down
- **I:** Flow: Add body override for HTTP Node
- **I:** Flow: Add raw json output to axis metadata event subscribe
- **I:** Flow: Add speedometer node

---

##### V2.0.24 – 26.10.2025
- **B:** Flow: Bend points on dragging moved all even if only one was selected.
- **I:** Flow: Imporve UI Contrast
- **I:** Flow: Add help dialog for nodes

---

##### V2.0.23 – 25.10.2025
- **B:** Flow: Bend points not moving when use selection.
- **I:** Flow: Bend points adding on left mouse done instead of click, Higlight edge connector on hover.
- **B:** Flow: Axis event create, now waits for completion callback.
- **I:** Flow: Axis event create no use the input name for the key in the event instead of the ID.
- **I:** Flow: Add node action toolbar text under the icon.
- **I:** Flow: Counter/Fifo/Lifo node, add edge detection
- **I:** Flow: Add panic recover on node exectuion, Increase over all stability for exectuion
- **I:** Flow: Axis output port dialog improved, when there is not output founded.
- **I:** Flow: General stability Improvments
---

##### V2.0.14 – 24.10.2025
- **B:** Flow: Compare node had invalid output type
- **I:** Flow: Tooltip for node disconnection added
- **I:** Flow: Manual Input node shows the value state
- **C:** Flow: Remove Lock funcion from floating bar

---

##### V2.0.13 – 23.10.2025
- **B:** Flow: Axis Meta Create note now allow stateless and statefull events
- **F:** Flow: Timer Sequence Node

---

##### V2.0.11 – 23.10.2025
- **F:** Flow Feature Release
- **C:** Remove AWS integration from SD-Card feature
- **C:** Old event based features removed (Camera Bridge, Timed Sequence, Sunset and Sunrise, Countdown)
- **I:** Reduce application footprint

---

##### V1.1.3 – 28.10.2025
- **F:** Add builds for devices without video 
- **F:** Add `LoggingSink` to emit metrics to application logs for diagnostics.
- **I:** Unknown metadata datatypes now default to string so all keys are preserved in pipeline output.
- **B:** Add SD reliability fixes and guards around non-VDO / non-storage builds to prevent runtime errors when features are disabled.
- **B:** Fix models to align with VDO decoupling and new build variants.
- **B:** Streaming endpoints now always return consistent JSON error objects instead of plain responses when the sink type is incorrect or on failures.
- **C:** Add AXIS OS 12 build steps for CV25, ARTPEC7, and ARTPEC8 (video and non-video variants).
- **C:** Update primary `manifest.json` (schemaVersion bump); add `manifestv11.json` for AXIS OS 11 targets; apply version bumps across manifests for interim releases.

