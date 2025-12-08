# Changelog

##### Legend  
- **I:** Improvement  
- **B:** Bugfix 
- **F:** New Feature  
- **C:** Change  

---

##### V2.1.5 - 09.12.2025
- **I:** Flow: Add Composite Nodes
- **I:** Flow: Add Teleport Nodes
- **I:** Flow: UI Improvement

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

---

##### V1.0.95 – 05.09.2025
- **B:** Websocket issue with 12.5 reverse proxy manifest modification

---

##### V1.0.94 – 23.07.2025
- **I:** Add SMS Eagle for SMS and Voice calls to notifications feature.

---

##### V1.0.93 – 04.07.2025
- **I:** Use axparmenter to obtain resolution and number of image configurations.

---

##### V1.0.92 – 02.07.2025
- **C:** Increase max. resolution (ui validation) for timelaps generation.

---

##### V1.0.91 – 23.06.2025
- **C:** Remove Event Countdown feature, could be used with older versions.
- **C:** Remove MQTT UI Defaults.
- **C:** Made non present of SD-Card more verbose, not show error toast message.
- **C:** Update open source licenses.
  
---

##### V1.0.90 – 21.06.2025
- **I:** Unify SD-Card Managment for all features, now it is more reslilent to SD-Card failures.
- **I:** Adding SD-Card cache for metadata dispatcher
- **I:** Adding MQTT sink for metadata dispatcher
- **I:** Adding feature start watchdog, in case of camera starts and external services prevent the feature from start.
- **I:** Improve stop code for some features.
- **I:** Adding SD-Card info to webui when one exists.
  
---

##### V1.0.89 – 13.06.2025
- **I:** Add delete button for metrics database (SD-Card)
- **I:** Split .eap builds into cv25, artep6, artpec7, metadata scene etc..

---

##### V1.0.88 – 12.06.2025
- **I:** Only transmit scene analytics when observations exists
- **B:** Fix consolidated issue that only one was possbile to add
  
---

##### V1.0.86 – 11.06.2025
- **I:** Add Metadata scene description analytics metric
- **I:** Add Metadata consolidated analytics metric
- **I:** Add JSON export for metrics export when stored on SD-Card
- **C:** Split artpec-builds into artep6 and artpec7+
  
---

##### V1.0.85 – 10.06.2025
- **B:** Fix RAM issue when downloading large meta data from SD-Card (CSV), now it use streams and DB operate in WAL mode.
- **C:** Correct sorting on metrices in metadata dispatcher (UI)
- **I:** Add PTZ movment metric
  
---

##### V1.0.82 – 06.06.202
- **F:** Add image to FTP feature
- **F:** Update goxis lib fixes SD-Card release callback issue
- **I:** Update golang version to latest

---

##### V1.0.73 – 06.06.2025
- **I:** Add loading indicator for csv download on sd-card metadata dispatch

---

##### V1.0.72 – 06.06.2025
- **I:** Add SD-Card sink to metadata dispatcher

---

##### V1.0.71 – 03.06.2025
- **B:** Fix an issue with event countdown that can lead into deadlock when stopping multiple times fast.

---

##### V1.0.7 – 01.06.2025
- **I:** Improve Image loading for SD-Card feature
- **B:** Fix image deletion regarding to database entries for SD-Card feature (File was deleted but not DB entrie)

---


##### V1.0.6 – 25.05.2025
- **I:** Add UI Popup for counting rule occupancy in area

---

##### V1.0.5 – 22.05.2025
- **I:** Add config import / export

---

##### V1.0.4 – 22.05.2025
- **I:** Add seven.io voice ringtime

---

##### V1.0.3 – 22.05.2025
- **I:** Add seven.io balance check before perform api call (sms, voice)

---

##### V1.0.2 – 22.05.2025
- **I:** Add doc link for event trigger component
- **B:** Fix race condition in countdown feature

---

##### V1.0.1 – 22.05.2025
- **C:** 12.4 SDK Adding to build pipeline

---

