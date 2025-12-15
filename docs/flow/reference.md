# Flow Reference

This document gathers every Flow-related control, helper, and node so that a UI-only user understands what is available, how it behaves, and what to look for while the graph is running.

## Workspace controls

### Live vs. edit mode
- **Start / Stop Execution**: The floating button in the action bar toggles live mode. Starting posts `/flow/start`, animates edges, freezes node movement, and streams runtime updates via the `flow` websocket. Stopping posts `/flow/stop`, saves the graph (`/flow/save` is called automatically), and returns you to edit mode so you can move nodes again.
- **Execution feedback**: The top-right stats card shows the last execution time when it exceeds ~45 ms. A yellow banner appears whenever the websocket reconnects so you know the runtime is temporarily offline.
- **Edge coloring**: While live, connections paint themselves based on the source output (booleans blink green/red, numbers use the `ioColor` theme). When you stop, the edges revert to grey and reconnectable nodes become interactive again.

### Canvas and helpers
- **Action toolbar**: The vertical bar near the top-left is draggable using the tiny grab handle. It holds the workflow buttons (Start/Stop, Add Node, Devices, IO Table, Composites, Fit to View, Export, Import, Introduction). Buttons that require edit mode (Add Node, Devices, Export/Import, Composites) are disabled while live; the IO Table only makes sense while live.
- **Safe mode**: The switch near the top-right toggles `safe_mode`. When enabled, invalid connections (wrong data types or already-booked inputs) show throttled warning toasts (`showAlreadyConnectedToast`, `ShowInvalidTypeToast` in the code). Turning it off silences those pop-ups without changing the wiring rules themselves.
- **Helper lines**: The helper-lines switch activates the alignment helpers defined in `helperLines.js`. Dragging a node near another displays vertical/horizontal guides within 5 px and snaps the node edge to match, keeping large graphs tidy.
- **Introduction dialog**: If a saved graph is empty or you click the Introduction button, a tutorial dialog (`IntroductionDialog`) reopens to walk through the basics. It automatically triggers on first load when there is no graph yet (`state.emtpyVisitDialog`).

### Node selection and properties
- **Add Node catalog**: Click "Add Node" to open the NodeSelectorDialog. You can search, filter by tag chips (e.g., Logic, Axis, Time), and drag templates directly onto the canvas. Axis-specific nodes (Axis Output, Metadata Event Subscribe, Metadata Event Create, Teleport, JSON To Node) open an intermediate dialog so you can choose devices, ports, events, or the data that should drive the node before it is placed.
- **Per-node drawer**: Clicking a node in edit mode opens the side drawer (`NodeProperties`). Tabs cover Settings (custom UI components), Appearance (title, subtitle, color), Inputs, and Outputs. There is also a Help button on each node toolbar that launches the NodeHelp components (`showHelpDialog`), giving detailed usage hints without leaving the canvas.
- **Deletion & composites**: Selecting nodes and pressing Delete (or clicking the Delete toolbar button that appears when two or more nodes are selected) queues a confirmation dialog before the nodes vanish. The "Create Composite" button appears automatically when at least two nodes are selected and connected; it copies the relevant graph, exposes inputs/outputs (using `buildExposedOptions`), and opens the composite dialog so you can name, tag, and describe the reusable unit.

### Device and composite management
- **Device dialog**: The Devices button opens `DeviceDialog`. From there you can add Axis devices with Name, IP/hostname, protocol/ports, username/password (min 3 characters), auth type, video channel, and the "Non-video device" flag. Non-video devices disable RTSP/resolution fields and move to "No Video" chips. The dialog can fetch a snapshot (`Get Snapshot`/`Test Connection`) before saving, and you can delete devices or edit credentials as needed.
- **Composite manager**: "Composites" opens a list of saved composites. You can import/export JSON definitions, delete old composites, and see their descriptions. When you re-save a composite, the list refreshes so the new version appears in the Add Node catalog with the chosen icon, tags, and category.

### Persistence, import/export, and diagnostics
- **Save/load lifecycle**: Flow calls `/flow/load` on mount and restores the last saved graph, including the previous run state (the server returns `run_state` and the UI re-enters live mode if needed). When you stop execution, `stopGraph(true)` first posts `/flow/save`, so the exact layout and node data are persisted on disk.
- **Graph import/export**: Export downloads the current graph as JSON (`exportGraph`), while Import uses a hidden file input so you can drop a `.json` file (`handleImportFileChange`). Import is blocked during live mode; the UI warns you to stop execution before loading.
- **Logging**: Runtime log messages arrive via the websocket (`ws.onmessage` handles messages with `type === 'log'`) and feed `state.loggingMessages`. Nodes such as the Logging node consume that array to show level, timestamp, node name/ID, and the message itself. Keeping a Logging node in the graph lets you see backend notes while you tune HTTP calls or Axis rules.
- **IO Table**: When the graph is running, click "IO Table" to open a compact table showing every node, input/output name, data type (bool/int/float/string), and the current state. It groups rows by node title and highlights boolean states with colored dots.

### Dialogs that appear before the node is placed
- **Axis Output port selection (`handleAxisOutputNode`)**: The Axis Output node opens `AxisAddDialog` before placement so you can pick the device, toggle which physical ports to expose, and reuse the stored credentials/resolutions.
- **Axis Metadata event subscribe (`handleAxisMetaData`)**: When adding the metadata subscribe node, a dialog lets you select the target event topic and device before the node hits the canvas. This ensures the dynamic outputs line up with the event payload (`createIoFromAxEvent` builds per-field IO definitions).
- **Metadata event create warning (`state.show_axis_create_dialog`)**: Creating metadata events is only visible to Axis rules when the flow is live; the dialog reminds you that the event exists only during execution.
- **JSON To Node**: The JsonToNode dialog captures the sample JSON you want to split, presents dynamic key names in Settings, and then the node itself emerges with the generated output handles (`handleJsonToNodeAdd`).
- **Teleport**: Adding a Teleport node prompts you to pick source outputs to mirror (`handleTeleportNode`). Once placed, it automatically wires copies of those edges so you can fan out a signal without drawing long wires.

## Execution engine (ax_msf Flow)
- Flow execution lives in the Go backend located under `ax_msf/flow`. When you hit Start the UI calls `/flow/start`, which spins up the ticker loop, topsort the graph (`node.TopologicalSort`), and begins evaluating nodes via the registered `executors`. Every clock tick (`evalTicker`) re-evaluates nodes, pushes changes to `/flow`’s websocket, and streams the latest graph plus execution time to the web UI (`type='graph'` payloads contain `flow_graph` and `exec_time`).
- Runtime events (errors, warnings, info) arrive with `type='log'` and land in the `state.loggingMessages` array. Bad nodes are wrapped in panic recovery so you still get an explanation rather than a hard crash.
- Composites are saved in `/flow/composites` (see `composites.go` and `templates.go`). When you create or import a composite, the backend exposes it as a template (`BuildCompositeTemplates`) with its own inputs, outputs, icon, and tags so the UI can render it like any stock node.
- The websocket stream also sends `ping` messages to keep the connection alive and shows a reconnect message (`reconnect_msg`) if the socket closes unexpectedly.

## Node library

The Flow canvas is backed by node templates coming from `ax_msf/flow/templates.go`. The sections below walk through every template you can drag from the Add Node catalog, grouped by their everyday purpose. Each entry lists the internal type, the default inputs/outputs, and the knobs you can tweak.

### Axis & device nodes

#### Axis Live Stream (`AxisRtspStream`)
- **Purpose**: Displays the live RTSP feed from an Axis device. While the node itself does not expose outputs, it shows a video window with "Connecting", "Error", and "Stream not connected" overlays when the flow is stopped (see `AxisRtspStreamNode.vue`).
- **Inputs**: none by default, but when the overlay flag (`extra_data.overlay_enabled`) is true or ROI/Observations/Tripwire data arrives, the node dynamically exposes handles named `ROI`, `Observations`, and `Tripwire`. These JSON inputs let you draw the overlay polygon, bounding boxes, or tripwire lines on top of the stream.
- **Outputs**: none.
- **Properties**: Choose the device/resolution in the Axis dialog, including username/password, RTSP port, channel, and whether overlay inputs should be used. The node retries every 5 seconds after errors (`state.streamError`), shows custom error text from the server, and colors bounding boxes using `colorForKey`.

#### Axis Device Log (`Syslog`)
- **Purpose**: Writes human-readable text into the Axis device syslog over VAPIX.
- **Inputs**: `Write` (boolean trigger), `Message` (string message).
- **Outputs**: none.
- **Notes**: The dialog uses the Axis device list (same Device dialog) to reuse stored credentials.

#### Axis Metadata Event (Subscribe) (`MetaDataEventSubscribe`)
- **Purpose**: Connects to Axis metadata event streams on a device and pushes every payload into the flow as node outputs.
- **Inputs**: `Enable` (boolean, only processes when true).
- **Outputs**: `Connected`, `Error`, `New Event`, `JSON`, plus the fields of the selected Axis event (each field becomes a handle when you choose the event via the dialog that opens before placement).
- **Notes**: Add the node and pick the specific metadata topic through the dialog. The node updates its outputs dynamically based on the metadata payload shape (`createIoFromAxEvent` dynamically generates handles).

#### Axis Metadata Event (Create) (`MetaDataEventCreate`)
- **Purpose**: Defines a custom Axis metadata event that becomes available to Axis rules or other ONVIF consumers while the flow runs.
- **Inputs**: `Send` (boolean trigger).
- **Outputs**: `Error`.
- **Properties**: Set the event name and choose stateless vs. stateful behavior; expose additional custom fields via the properties drawer. The UX warns (via `state.show_axis_create_dialog`) that Axis only sees these events while the graph is live.

#### Axis Parameter (`AxisParameter`)
- **Purpose**: Reads any AXParameter key through the ACAP parameter handler and exposes the latest string value together with any error messages.
- **Inputs**: `Parameter Name` (string) – the AXParameter path you want to read, for example `Properties.System.SerialNumber` or `Image.I0.Appearance.Resolution`. Wire other nodes to build dynamic keys or type a fallback in the properties drawer.
- **Outputs**: `Value` (string) – last successful parameter value; `Error` (string) – populated when the request fails (missing permissions, typos, or the device is unreachable).
- **Notes**: The properties drawer includes a dedicated field so you can keep reading the same parameter even when nothing is connected. When another node drives the `Parameter Name` input the property field is ignored.

#### Axis Outputs (`AxisOutput`)
- **Purpose**: Fires VAPIX commands that toggle physical Axis output ports (relays/digital outs).
- **Inputs**: configured per device inside the Axis dialog (the node populates inputs on the fly when selecting the device and available ports).
- **Outputs**: `Success`, `Status`, `Error`.
- **Tips**: Recreate the node after changing device credentials or port mappings so the inputs stay in sync with the hardware.

#### Vaxtor Cloud ANPR (`VaxtorCloudAnpr`)
- **Purpose**: Captures a snapshot from a device, uploads it to Vaxtor Cloud, and returns plate/vehicle metadata.
- **Inputs**: `Execute`, `Reset`.
- **Outputs**: `Success`, `Plate Found`, `Number`, `Country`, `State`, `Category`, `Read Confidence`, `Vehicle Make/Model/Color/Class`, `Status`, `Error`, `Executed`, `Image ID` (string that references the cached JPEG produced for the request).
- **Properties**: Configure the OAuth2 client credentials, API URL, timeout, whether to reuse a device/resolution snapshot (`extra_data.use_resolution`), and optionally pick a resolution from the Device dialog. The helper dialog explains which fields are required. Pass the `Image ID` output to a `JpegViewer` to preview the snapshot without shipping the binary through the websocket.

#### Image Snapshot (`ImageSnapshot`)
- **Purpose**: Captures a JPEG snapshot from the selected Axis device so you can use that image elsewhere in the graph without transporting raw bytes over the websocket.
- **Inputs**: `Capture`, `Reset`.
- **Outputs**: `Success`, `Image ID`, `Error`, `Executed` (rising edge when the capture completes).
- **Properties**: Pick a device in the same dialog used for Axis outputs/streams, and optionally force `use_resolution`+ `resolution_idx` to grab a specific stream size. The emitted `Image ID` can feed downstream nodes such as `JPEG Viewer` or the HTTP node when you want to reference the image again.

#### JPEG Viewer (`JpegViewer`)
- **Purpose**: Renders a cached JPEG by asking the backend for the image referenced by `Image ID`, avoiding huge websocket payloads.
- **Inputs**: `Image ID` (string from `Http`, `Vaxtor Cloud ANPR`, `Image Snapshot`, or any node that exposes a cached image identifier).
- **Outputs**: none (the node only paints the image on the canvas).
- **Properties**: Adjust the width/height, toggle the border, and let the node talk to the shared in-memory image store (10 images max, 30 MB total) over the regular HTTP API. Only one viewer per `Image ID` exists in memory—when the ID expires or a new image replaces it, the viewer automatically stops requesting the obsolete image.

#### License Plates (`LicensePlateList`)
- **Purpose**: Matches incoming plate strings against the whitelist managed in the standalone License Plate List feature.
- **Inputs**: `Process` (boolean trigger), `Plate` (string), `Reset` (boolean).
- **Outputs**: `Error`, `Matched Plate`, `In List`, `Not In List`, `Expired`, `Not Valid Yet`, `Processed`, `Total Plates`, plus a boolean output per configured tag (`T.01`, `T.02`, …).
- **Notes**: The node reads the shared license plate database, honors validity windows, and sets the tag outputs for any matching entry. Use the `License Plate List` docs to manage the table.

### Inputs & manual controls

#### Manual Input (`InputManual`)
- **Purpose**: Provides a typed constant (boolean/int/float/string) that you can tweak by clicking the DataType chip in the title or the Data Type selector in the properties panel.
- **Inputs**: none.
- **Outputs**: `Value` (defaults to `true`, datatype adjusts when you change the selector).

#### Enable Input (`Enable`)
- **Purpose**: Simple helper that always outputs `true`. Use it as a permanent enable signal for timers, HTTP requests, or to gate composite execution.
- **Inputs**: none.
- **Outputs**: `True` (boolean).

#### Button (`Button`)
- **Purpose**: Acts like a physical button. The output goes high while you hold the node pressed and returns to false on release.
- **Inputs**: none; the node handles its own `pressed` state in `extra_data`.
- **Outputs**: `Active` (boolean).

### Timing & sequencing

#### In Time (`InTime`)
- **Purpose**: Yields `true` inside a configured daily window (08:00-17:00 Monday–Friday by default).
- **Inputs**: none.
- **Outputs**: `In Time` (boolean).
- **Properties**: Use the timer panel to set start/end times, pick the allowed days, and toggle the `valid` flag for quick overrides.

#### Debounce (`Debounce`)
- **Purpose**: Filters noisy boolean signals. The output only changes when the input stays stable for the configured duration.
- **Inputs**: `Input`, `Reset`.
- **Outputs**: `Value`, `Elapsed` (time spent waiting, in milliseconds).
- **Properties**: Choose `unit` (ms/s/m) and `amount` for the wait time; Reset clears the timer.

#### Edge Detection (`Edge`)
- **Purpose**: Emits a one-cycle `Value` when a boolean input crosses a rising or falling edge.
- **Inputs**: `Value`.
- **Outputs**: `Value`.
- **Properties**: Pick `rising` or `falling` mode; the title chip flips between teal/blue to remind you of the direction.

#### Pulse Buffer with Release (`ImpulseStorage`)
- **Purpose**: Queues up pulses until a release signal frees them, useful to synchronize multiple consumers.
- **Inputs**: `Add`, `Release`, `Clear`.
- **Outputs**: `Pulse`.
- **Properties**: Set the buffer depth (`buffer_amount`) in the inspector and watch the live buffer counter in the node title (`actual_buffer`).

#### Next Cycle Pulse (`NextCycle`)
- **Purpose**: Detects a rising edge and pushes the pulse into the next execution cycle (useful when an immediate response would be too early).
- **Inputs**: `Trigger`.
- **Outputs**: `Pulse`.

#### One Shot Pulse After (`OneShotPulseAfter`)
- **Purpose**: Emits a delayed pulse when `Trigger` rises.
- **Inputs**: `Trigger`.
- **Outputs**: `Pulse`, `Elapsed Time`.
- **Properties**: Set the delay amount and unit (ms/s/min) via the shared `Timer` properties UI.

#### Timer On Delay (`ToFN` – TON)
- **Purpose**: Outputs `Active` only after `Enable` stays true for the configured duration. `Elapsed Time` tracks progress.
- **Inputs**: `Enable`.
- **Outputs**: `Active`, `Elapsed Time`.
- **Properties**: Choose the time amount/unit just like the other ToFN variants.

#### Timer Off Delay (`ToFN` – TOF)
- **Purpose**: Goes high immediately when `Enable` goes true, then stays high for the delay after the input falls.
- **Inputs/Outputs**: same as TON.

#### Pulse Timer (`ToFN` – TP)
- **Purpose**: Emits a fixed-length pulse for every rising edge of `Enable`.
- **Inputs/Outputs**: same as TON (only the timing behavior changes).

#### Pulse Generator (`ToFN` – Pulse Generator)
- **Purpose**: While `Enable` stays true, emits periodic one-cycle pulses spaced by the configured duration.
- **Inputs**: `Enable`.
- **Outputs**: `Active`.

#### Step Sequencer (`StepSequencer`)
- **Purpose**: Advances through `steps` outputs each time `Step` goes true. Only one output is high at a time.
- **Inputs**: `Step`.
- **Outputs**: `Step 1…Step N` (default 4 boolean handles).
- **Properties**: Control the step count and whether the sequence wraps around.

#### Timer Sequence (`TimerSequence`)
- **Purpose**: Chains multiple timer slots while enabled, toggling the corresponding output per slot.
- **Inputs**: `Enable`.
- **Outputs**: `Elapsed Time`, `Timer Index`, and boolean outputs for each configured timer step (Timer 1, Timer 2, …).
- **Properties**: Define durations in milliseconds/seconds/minutes and labels for each slot.

#### Counter (`Counter`)
- **Purpose**: Tracks an integer value that can increment, decrement, and reset based on pulses.
- **Inputs**: `Up`, `Down`, `Reset`.
- **Outputs**: `Value`.

### Logic & math

#### Logic gates (`Logic`)
- **Purpose**: Build AND, NAND, OR, NOR, XOR, XNOR, and NOT gates. Each operator appears separately in the Add Node catalog but reuses the same template with different `internal_node_type`.
- **Inputs**: `A` and `B` (booleans). `NOT` only uses `A`.
- **Outputs**: `Result` (boolean).
- **Notes**: Safe mode highlights invalid combinations, and the node help dialog explains the truth tables.

#### Compare (`Compare`)
- **Purpose**: Compares A and B using operators such as `==`, `!=`, `>`, `<`, `>=`, `<=`, `contains`, `startswith`, and `endswith`.
- **Inputs**: `A`, `B`.
- **Outputs**: `Result`.
- **Properties**: Select the operator and the data type (int/float/string) so the node will coerce inputs accordingly before evaluating.

#### Value Selector (`ValueSelector`)
- **Purpose**: Routes either `Input 1` or `Input 2` to the output based on the boolean `Select` input.
- **Inputs**: `Select`, `Input 1`, `Input 2`.
- **Outputs**: `Value`.
- **Properties**: Pick the shared data type (int/float/string) in the properties panel; the node enforces the same type on both inputs and the output.

#### Multiplexer (`Multiplexer`)
- **Purpose**: Picks one of many value inputs by number and reports which index was selected.
- **Inputs**: `Index` (integer) plus `Value-1…Value-N`.
- **Outputs**: `Value`, `Selected Index`.
- **Properties**: Add/remove data inputs from the settings dialog and choose their datatype.

#### Move (`Move`)
- **Purpose**: Passes the incoming value through when `Enable` is true, acting as a gated buffer.
- **Inputs**: `Enable`, `Value`.
- **Outputs**: `Value`.

#### Math (`Math`)
- **Purpose**: Adds, subtracts, multiplies, or divides `A` and `B` once `Enable` is true.
- **Inputs**: `Enable`, `A`, `B`.
- **Outputs**: `Done`, `Result`.
- **Properties**: Choose the operation from the dropdown (add/sub/mul/div).

#### Advanced Math (`MathAdvanced`)
- **Purpose**: Runs an advanced math function (ABS, SQRT, SIN, COS, TAN, SIGN) on a single `Value`.
- **Inputs**: `Enable`, `Value`.
- **Outputs**: `Done`, `Value`.
- **Properties**: Pick the function from the properties dialog; the node keeps output precision and respects the `Enable` gate.

#### Flip-Flop (`FlipFlop`)
- **Purpose**: Stores one bit and reacts to `Set`/`Reset` signals (`mode` can be RS or SR).
- **Inputs**: `Reset`, `Set`.
- **Outputs**: `Value`.
- **Properties**: Switch modes and the input labels update automatically.

#### Value Latch (`ValueLatch`)
- **Purpose**: Memorizes the last `Value` seen when `Set` is high and releases it until `Reset` clears the latch.
- **Inputs**: `Value`, `Set`, `Reset`.
- **Outputs**: `Value`.

#### Edge Value Mux (`EdgeValueMux`)
- **Purpose**: On a rising trigger the node pulses and forwards the paired string payload.
- **Inputs**: four trigger/value pairs (`Trigger 1`/`Value 1`, … `Trigger 4`/`Value 4`).
- **Outputs**: `Pulse` (true for one cycle) and `Value` (string that matched the rising trigger).

#### Conversion helpers
- **Int to Bool** (`IntToBool`): Converts non-zero integers to `true`. Inputs: `Value` (int). Outputs: `Bool` (boolean).
- **Bool to Int** (`BoolToInt`): Maps boolean inputs to `0`/`1`. Inputs: `Value` (boolean). Outputs: `Int` (integer).

### Data & transformation

#### Queue FIFO (`Fifo` with `InternalNodeTypeFIFO`)
- **Purpose**: Enqueues integer values and pops them in FIFO order; useful as a simple buffer or for smoothing pulses.
- **Inputs**: `Value`, `Add`, `Pop`, `Reset`.
- **Outputs**: `Value`, `Has` (true when the queue is not empty), `Count`.
- **Properties**: Choose the maximum list length; overflow drops the oldest value.

#### Queue LIFO (`Fifo` with `InternalNodeTypeLIFO`)
- **Purpose**: Stores values in Last-In First-Out order, useful for stack-like behavior.
- **Inputs/Outputs**: same as FIFO but the most recently added value is popped first.

#### Data Table (`DataTable`)
- **Purpose**: Provides eight independent registers that you can wire into, inspect, and edit; the defaults are simple integer outputs.
- **Inputs**: none (values are manipulated via the properties drawer or via connected nodes that drive the outputs).
- **Outputs**: Eight numeric handles (`O.01…O.08`) you can rename in the property panel.

#### JSON Key Extractor (`JsonKeyExtract`)
- **Purpose**: Pulls a single key (dot notation supported) from a JSON string and flags whether it was found.
- **Inputs**: `Extract`, `String`, `Reset`.
- **Outputs**: `Value`, `Found`.
- **Properties**: Enter the key path in the properties panel and enable preview mode to see the parsed value before pressing `Extract`.

#### JSON To Node (`JsonToNode`)
- **Purpose**: Splits a JSON object into individual outputs, one per top-level key.
- **Inputs**: `Process`, `Reset`, `JSON`.
- **Outputs**: `Processed`, `Error`, plus dynamically generated key handles you define via the sample JSON field before placement.

#### JSON Array Selector (`JsonArraySelector`)
- **Purpose**: Selects the JSON object at a specific zero-based index from a JSON array string.
- **Inputs**: `Process`, `JSON Array`.
- **Outputs**: `Processed`, `Found`, `Error`, `Item JSON`.
- **Properties**: Pick the index, piping in arrays stored elsewhere in the flow.

#### JSON Viewer (`JsonViewer`)
- **Purpose**: Formats JSON payloads for humans, complete with indentation and theming.
- **Inputs**: `JSON`.
- **Outputs**: none.
- **Properties**: Adjust indent, height, and color theme for the rendered box.

#### HTTP Request (`Http`)
- **Purpose**: Fires GET/POST/PUT/etc. requests with optional body overrides, reports the response, and caches JPEG bodies without shipping the raw bytes through the websocket.
- **Inputs**: `Execute`, `Reset`, `Body`.
- **Outputs**: `Success`, `Status`, `Body`, `Error`, `Executed`, `Image ID` (string produced whenever the backend detects a JPEG body so you can visualize it with the JPEG Viewer or reuse the image later).
- **Properties**: Configure URL, method, headers, body templates, authentication (none/basic/digest), TLS options, and optional device snapshot templates (`GenericData`). When `Image ID` is emitted you can drop it directly into `JpegViewer` or other display helpers instead of pushing megapixel blobs through the live graph.

#### Webhook Listener (`Webhook`)
- **Purpose**: Runs an HTTP/HTTPS endpoint that accepts Basic or Digest authentication, applies the configured rate limit (1–15 requests per second), and emits each payload plus optional query parameters into the flow with a stable response contract.
- **Inputs**: `Enable` (boolean gate), `Custom Response` (boolean flag), `Response Body` (string), `Response Code` (int). The first toggles the listener, while the remaining fields let you override the automatic `{ "success": ..., "message": ... }` reply per request.
- **Outputs**: `New Request` (boolean pulse), `Body` (string raw request body or pre-parsed payload), `Error` (string describing validation/auth failures), plus one string output per configured query parameter.
- **Properties**: Set the port (HTTP/HTTPS), path, TLS certificate/key, request rate limit, and auth credentials in the inspector. Query parameters defined here automatically generate matching string outputs so you can pick tokens like `device_id` or `token` without parsing them manually. The node always replies with the JSON body `{"success":bool,"message":string}` so clients know whether to retry.

### Visualization & debugging

#### Bool Debug Output (`BoolDebug`)
- **Purpose**: Displays the boolean value it receives with configurable icons, colors, and labels.
- **Inputs**: `Value`.
- **Outputs**: none.
- **Properties**: Set `true_icon`, `false_icon`, colors, and the text shown for each state; the properties panel exposes all of these controls.

#### String Debug Output (`StringDebug`)
- **Purpose**: Prints the latest string value for quick inspection.
- **Inputs**: `Value`.
- **Outputs**: none.

#### Value Debug Output (`IoDebug`)
- **Purpose**: Shows a live table of boolean/int/float/string inputs when the graph is running.
- **Inputs**: `Value - Bool`, `Value - Int`, `Value - Float`, `Value - String`.
- **Outputs**: none.

#### Logging (`Logging`)
- **Purpose**: Renders the runtime log feed (`state.loggingMessages`) with level chips, timestamps, node IDs, and descriptions.
- **Inputs**: none (the node subscribes automatically to the websocket log array).
- **Outputs**: none.

#### Oscilloscope (`Oscilloscope`)
- **Purpose**: Draws up to five live signals on a scrolling chart with configurable colors/ranges.
- **Inputs**: five generic handles (bool/int/float) by default.
- **Outputs**: none.
- **Properties**: Tune per-input colors, thresholds, and min/max values to keep the waveform readable.

#### Speedometer (`Speedometer`)
- **Purpose**: Visualizes a numeric input as a gauge with needle, segments, and color gradient.
- **Inputs**: `Value`.
- **Outputs**: none.
- **Properties**: Set `min`, `max`, `segments`, `needleColor`, `startColor`, `endColor`, and number formatting.

#### Text (`Text`)
- **Purpose**: Renders static text for labeling sections of the canvas or giving instructions.
- **Inputs**: none.
- **Outputs**: none.
- **Properties**: Change `text`, `size`, and `color` in the properties panel; general properties are disabled for this node so the entire panel is dedicated to the text itself.

### Advanced & utility

#### Lua Script (`Lua`)
- **Purpose**: Runs custom Lua code that can read inputs and write outputs on demand.
- **Inputs**: `Execute`, plus two additional boolean inputs (`I.02`, `I.03`) available for your script.
- **Outputs**: `O.01` (reserved for errors), `O.02`, `O.03`.
- **Properties**: Paste your Lua script in the properties panel; the default snippet shows how to access `inputs[]`, `outputs[]`, and the persistent `state` table. The node ships with helper functions such as `json_encode`, `json_decode`, `time()`, and `date()`.

#### Teleporter (`Teleport`)
- **Purpose**: Mirrors a signal from one node to another without drawing long wires.
- **Inputs/Outputs**: none initially. Before the node is placed, the teleport dialog (`handleTeleportNode`) asks which outputs to copy. It then auto-creates hidden edges between the selected outputs and the teleport node, so the remote node sees the same state.

#### Composite nodes (`Composite*`)
- **Purpose**: Reusable graphs that appear in the Add Node dialog once you save them via the composite dialog.
- **Inputs/Outputs**: defined per composite; you pick which internal nodes’ handles become exposed.
- **Properties**: From the action bar you can open the Composite Manager to export/import/delete composites. When you create a composite, the UI automatically generates default `external_id`s (`I.XX`, `O.XX`), lets you categorize/tag the composite, and stores the graph plus version metadata.
