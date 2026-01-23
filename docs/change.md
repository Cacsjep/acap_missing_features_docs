# Changelog

##### Legend
- **I:** Improvement
- **B:** Bugfix
- **F:** New Feature
- **C:** Change

##### V3.1.8 - 23.01.2026
- **I:** Parking Management: Added embedded Plate List tab for managing license plates directly within Parking Management
- **I:** Parking Management: Help dialogs for Zone configuration, IO Port Actions, and Event Rules
- **I:** Parking Management: Devices button in toolbar for quick access to device management
- **I:** Parking Management: Consistent section styling across all tabs (header/content layout)
- **I:** Parking Management: Live view top bar now shows zone capacity per zone, overtime count, and utilization percentage
- **I:** Parking Management: Plate Log improvements - ID column, better column layout, help dialog with event state explanations
- **I:** Parking Management: Parked Vehicles improvements - header row, combined Duration/Progress into "Parking" column, help dialog
- **I:** Parking Management: Zone panels redesigned from expansion panels to section layout
- **I:** Parking Management: Tags automatically refresh when leaving Plate List tab
- **I:** Parking Management: Added Zone Full and Unauthorized event types for IO Rules
- **C:** Parking Monitor: Removed because now parking managment do this.

##### V3.1.6 - 22.01.2026
- **I:** Parking Management: Camera offline status now shows red indicator instead of gray
- **I:** Parking Management: Real-time camera connection status updates when cameras disconnect/reconnect
- **I:** Parking Management: Devices sorted by ID for stable UI ordering
- **I:** Parking Management: Plates show correct tags for "exit without entry" events
- **B:** Parking Management: Fixed CSS overflow - zones can now scroll properly when expanded
- **B:** Parking Management: Fixed plate date validation (FromDate/ToDate now checked)
- **B:** Parking Management: Fixed zone full behavior - entries are now created even when zone is full
- **B:** Parking Management: Fixed multi-zone camera support - events processed for all zones

##### V3.1.5 - 21.01.2026
- **F:** Parking Management: New standalone feature for comprehensive parking lot management
- **F:** Dashboard: Media Image widget for displaying static images from SD card media folder
- **F:** Dashboard: Media Video widget for looping videos from SD card media folder
- **F:** Dashboard: YouTube widget for embedding YouTube videos
- **F:** Dashboard: Clock widget for displaying time with timezone support
- **F:** Dashboard: Markdown widget for formatted text display
- **F:** Dashboard: Weather widget for current weather using Open-Meteo
- **B:** WebSocket: Fixed server-side connection leak - connections now properly close when client disconnects
- **I:** Notification Service: Replaced custom buttons with Vuetify components for consistency
- **I:** Notification Service: Ring time validation extended from 5-30 to 5-60 seconds
- **I:** Logging: All WebSocket-related messages changed from INFO to DEBUG level

##### V3.1.3 - 15.01.2026
- **B:** ParkingTimes: Fixed WebSocket connection not closing on component unmount (memory leak)
- **B:** OccupancyMultiple: Fixed WebSocket connection not closing on component unmount (memory leak)
- **B:** Spot Color Detector: Fixed endless loading state when restarting with no triggers configured

##### V3.1.1 - 14.01.2026
- **F:** Restored: Notification Service feature for SMS/Voice alerts via Seven.io, SMSEagle, and Teltonika
- **F:** Restored: Occupancy Monitor feature for multi-zone occupancy counting
- **F:** Restored: Parking Times feature for license plate parking duration tracking
- **F:** Restored: Sunrise & Sunset Events feature for location-based daylight events
- **I:** Spot Color Detector: Added drag handle to circle/rect menus for repositioning
- **I:** Occupancy Monitor: Configuration panel hidden for users with read-only permission
- **I:** Parking Times: Configuration panel hidden for users with read-only permission

##### V3.0.9 - 13.01.2026
- **F:** Settings: User management now supports Default Dashboard and Allowed Dashboards per user
- **F:** Dashboard: Export and Import dashboards via More menu (for users with edit permission)
- **I:** Settings: Delete confirmation dialogs for users and roles
- **I:** Settings: Users now have single role instead of multiple roles
- **I:** Settings: Username duplicate validation on user creation
- **I:** Flow: Parking Monitor node properties reorganized into logical sections (Overstay Detection, Data Cleanup, Exit Cleanup)
- **C:** Flow: Parking Monitor date format changed to German format (DD.MM.YYYY HH:MM:SS)
- **I:** Dashboard: Table widget date renderer now supports both German and ISO date formats
- **B:** Dashboard: String Input widget fixed race condition where server updates could overwrite user input while typing
- **C:** Dashboard: String Input widget value is now sent only on Enter key or clicking away (not on every keystroke)
- **F:** Dashboard: String Input widget added Style option (Outlined, Filled, Underlined, Solo, Solo Filled, Plain)
- **C:** Device Discovery: Devices are no longer auto-selected when global credentials are entered, users must manually select devices to add

##### V3.0.8 - 11.01.2026
- **B:** Flow: Cache node now correctly serializes values (was showing empty objects)
- **B:** Flow: JSON Get/Set/Remove nodes now correctly pulse `done` only on rising edge (was stuck true)
- **I:** Video Components rewritten to use webcodecs instead of webrtc to elimintate NAT (Spot Color, Metadata Trigger, Flow AXIS Live Stream, Dashboard Live Stream)
- **I:** Logging: WebSocket close errors (1000/1001) now logged at DEBUG level instead of ERROR
- **I:** Logging: WebCodecs ping/write errors now logged at DEBUG level

##### V3.0.7 - 09.01.2026
- **F:** Settings: New "UI" tab with configurable toast message position and duration
- **F:** Dashboard: Table widget now supports optional CSV export button
- **F:** Dashboard: Table widget now supports optional search field for filtering rows
- **B:** Flow: SQL Connection node no longer reconnects every cycle (fixed 2-3ms idle overhead)
- **B:** Flow: Fix JSON float64 to int type conversion in rtmp node
- **F:** Flow: Parking Monitor Exit plate delete input and methods added
- **B:** Flow: Parking Monitor auto timer operations with same interval no longer block each other
- **B:** Flow: Parking Monitor input index alignment after Label input was added
- **B:** Flow: Teleporter node copy now correctly copies hidden edges
- **B:** Discovery: Auth detection now tries HTTPS first to correctly detect "Recommended" auth policy on newer Axis devices

##### V3.0.6 - 09.01.2026
- **I:** Dashboard: AXIS Live Stream widget now shows errors in display area instead of toast notifications
- **B:** Flow: Fix RTSP URL generation for credentials containing special characters

##### V3.0.4 - 08.01.2026
- **F:** Flow: Add label support for license plate list entries
- **I:** Security: Rate limiting for login attempts
- **I:** Security: Enhanced password change validation
- **I:** Auth: Loading state for authentication initialization
- **C:** Flow: Removed IntroductionDialog and tutorial videos (moved to YouTube channel)
- **F:** Flow: Multi-flow support with tabbed interface (run multiple flows simultaneously)
- **F:** Flow: Add manual Save button to toolbar (in addition to auto-save on run)
- **F:** Flow: Add Channel Publisher/Subscriber nodes for cross-flow communication
- **F:** Flow: Add Analytics Metadata node
- **F:** Flow: Add Tags input to Parking Monitor for categorizing entries
- **F:** Dashboard: Add Static Text widget for labels and headers (no node required)
- **F:** Dashboard: Add String Input widget for text entry controls
- **F:** Spot Color Detector: Add Rectangle triggers for flexible region placement (handles lens distortion)
- **F:** Spot Color Detector: Add combined platform events (all_circles_state, all_rects_state) with individual color fields per trigger
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

