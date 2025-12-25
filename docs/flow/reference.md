# Flow Reference

This document covers every Flow-related control, helper, and node so you understand what is available and how it behaves.

## Workspace Controls

### Live vs. Edit Mode

- **Start / Stop Execution**: The floating button in the action bar toggles between modes. Starting the flow animates edges, locks node movement, and runs the automation. Stopping saves the graph and returns you to edit mode.
- **Execution feedback**: A stats card in the top-right shows execution time. A yellow banner appears if the connection temporarily drops.
- **Edge coloring**: While live, connections show colors based on data type (booleans glow green/red, numbers show different colors). When stopped, edges turn grey.

### Canvas and Helpers

- **Action toolbar**: The vertical bar near the top-left contains workflow buttons: Start/Stop, Add Node, Devices, IO Table, Composites, Fit to View, Export, Import, Introduction. Some buttons are disabled while the flow is running.
- **Safe mode**: The switch near the top-right shows warning messages when you try to make invalid connections (wrong data types or already-connected inputs). Turn it off to silence the warnings.
- **Helper lines**: Displays alignment guides when dragging nodes near each other, helping keep your graph tidy.
- **Introduction dialog**: Opens automatically on first use or when clicking the Introduction button to walk through the basics.

### Node Selection and Properties

- **Add Node catalog**: Click "Add Node" to browse available nodes. Search, filter by category tags, and drag nodes onto the canvas. Some nodes (like Axis nodes) open a setup dialog before placement.
- **Per-node drawer**: Click a node in edit mode to open the side panel with Settings, Appearance, Inputs, and Outputs tabs. Each node also has a Help button for usage hints.
- **Deletion & composites**: Select nodes and press Delete to remove them. When multiple connected nodes are selected, a "Create Composite" button appears to bundle them into a reusable group.

### Device and Composite Management

- **Device dialog**: Click "Devices" to add Axis devices with their connection details (IP, port, username, password, auth type, channel). You can test connections and manage device credentials here.
- **Composite manager**: Click "Composites" to view, import, export, or delete saved composite nodes.

### Saving and Exporting

- **Auto-save**: The flow saves automatically when you stop execution.
- **Graph import/export**: Export downloads your flow as a JSON file. Import loads a JSON file (only works when stopped). Users need the appropriate permission to see these buttons.
- **Logging**: Add a Logging node to see runtime messages including errors, warnings, and info from your nodes.
- **IO Table**: While running, click "IO Table" to see a live table of all node inputs/outputs and their current values.

### Node Setup Dialogs

Some nodes open a dialog before placement:

- **Axis Output**: Select the device and which physical ports to control
- **Axis Metadata Subscribe**: Choose the event topic and device
- **Metadata Event Create**: A reminder appears that these events are only visible while the flow is running
- **JSON To Node**: Enter sample JSON to generate output handles
- **Teleport**: Pick which outputs to mirror from other nodes

---

## Node Library

The sections below describe every node you can add, grouped by purpose.

### Axis & Device Nodes

#### Axis Live Stream

Displays a live video stream from an Axis camera.

- **Inputs**: Optional overlay data (ROI, Observations, Tripwire)
- **Outputs**: None
- **Properties**: Device selection, resolution, overlay settings

The stream retries automatically every 5 seconds if connection fails.

#### Axis Device Log

Writes messages to the Axis device system log.

- **Inputs**: `Write` (trigger), `Message` (text)
- **Outputs**: None

#### Axis SD Card Nodes

File operations on the camera's SD card:

| Node | Purpose | Key Outputs |
|------|---------|-------------|
| SD Card Status | Check card state | Present, Writable, Full |
| File Exists | Check if file exists | Exists |
| Write File | Create/overwrite file | Written |
| Append File | Add to file | Written |
| Read File | Read file contents | Data, File Not Exists |
| Remove File | Delete file | Removed |
| List Files | Get all files | Data (JSON), Count |
| Usage | Storage used | Used GB |

#### Axis Metadata Event (Subscribe)

Listens to Axis metadata events and exposes them as outputs.

- **Inputs**: `Enable`
- **Outputs**: `Connected`, `Error`, `New Event`, `JSON`, plus event-specific fields
- **Setup**: Select the event topic through the dialog when adding the node

#### Axis Metadata Event (Create)

Creates custom events visible to Axis rules while the flow runs.

- **Inputs**: `Send` (trigger), plus custom fields you define
- **Outputs**: `Error`
- **Properties**: Event name, stateless/stateful mode, custom fields

!!! warning
    Events are only visible to Axis rules while the flow is running.

#### Axis Parameter

Reads any Axis parameter value.

- **Inputs**: `Parameter Name` (e.g., `Properties.System.SerialNumber`)
- **Outputs**: `Value`, `Error`

#### Axis Outputs

Controls physical output ports (relays, digital outputs).

- **Inputs**: Configured per device/port
- **Outputs**: `Success`, `Status`, `Error`

!!! tip
    Recreate the node if you change device credentials or port mappings.

#### Vaxtor Cloud ANPR

Captures a camera snapshot and sends it to Vaxtor Cloud for plate recognition.

- **Inputs**: `Execute`, `Reset`
- **Outputs**: `Success`, `Plate Found`, plate details (Number, Country, State, Category, Confidence), vehicle details (Make, Model, Color, Class), `Status`, `Error`, `Executed`, `Image ID`

#### Image Snapshot

Captures a JPEG snapshot from an Axis device.

- **Inputs**: `Capture`, `Reset`
- **Outputs**: `Success`, `Image ID`, `Error`, `Executed`

Use the `Image ID` with JPEG Viewer or HTTP nodes.

#### JPEG Viewer

Displays a cached image on the canvas.

- **Inputs**: `Image ID` (from Image Snapshot, HTTP, or Vaxtor)
- **Outputs**: None
- **Properties**: Width, height, border

#### License Plates

Matches plates against the configured whitelist.

- **Inputs**: `Process`, `Plate`, `Reset`
- **Outputs**: `Matched Plate`, `In List`, `Not In List`, `Expired`, `Not Valid Yet`, `Processed`, `Total Plates`, plus tag outputs

---

### Inputs & Manual Controls

#### Enable Input

Always outputs `true`. Use as a permanent enable signal.

- **Outputs**: `True`

#### Button

Outputs `true` while pressed, `false` when released.

- **Outputs**: `Active`

---

### Timing & Sequencing

#### In Time

Outputs `true` during configured daily time windows.

- **Outputs**: `In Time`
- **Properties**: Start/end times, allowed days

#### Debounce

Filters noisy signals. Output changes only after input is stable for the set time.

- **Inputs**: `Input`, `Reset`
- **Outputs**: `Value`, `Elapsed`
- **Properties**: Time amount and unit

#### Edge Detection

Emits a pulse when input changes state.

- **Inputs**: `Value`
- **Outputs**: `Value` (pulse)
- **Properties**: Rising or falling edge mode

#### Pulse Buffer with Release

Stores pulses until released.

- **Inputs**: `Add`, `Release`, `Clear`
- **Outputs**: `Pulse`
- **Properties**: Buffer size

#### Next Cycle Pulse

Delays a pulse to the next execution cycle.

- **Inputs**: `Trigger`
- **Outputs**: `Pulse`

#### One Shot Pulse After

Emits a delayed pulse after trigger.

- **Inputs**: `Trigger`
- **Outputs**: `Pulse`, `Elapsed Time`
- **Properties**: Delay time and unit

#### Timer On Delay (TON)

Output goes true after input stays true for the configured duration.

- **Inputs**: `Enable`
- **Outputs**: `Active`, `Elapsed Time`

#### Timer Off Delay (TOF)

Goes true immediately, stays true for configured time after input goes false.

- **Inputs**: `Enable`
- **Outputs**: `Active`, `Elapsed Time`

#### Pulse Timer (TP)

Fixed-length pulse on each rising edge.

- **Inputs**: `Enable`
- **Outputs**: `Active`, `Elapsed Time`

#### Pulse Generator

Periodic pulses while enabled.

- **Inputs**: `Enable`
- **Outputs**: `Active`

#### Step Sequencer

Advances through outputs on each trigger. Only one output is active at a time.

- **Inputs**: `Step`
- **Outputs**: `Step 1` through `Step N`
- **Properties**: Number of steps

#### Timer Sequence

Runs through timed stages while enabled.

- **Inputs**: `Enable`
- **Outputs**: `Elapsed Time`, `Timer Index`, plus one output per timer stage
- **Properties**: Duration and label for each stage

#### Counter

Counts up/down on pulses.

- **Inputs**: `Up`, `Down`, `Reset`
- **Outputs**: `Value`

---

### Logic & Math

#### Logic Gates

Standard boolean gates: AND, NAND, OR, NOR, XOR, XNOR, NOT.

- **Inputs**: `A`, `B` (NOT uses only `A`)
- **Outputs**: `Result`

#### Compare

Compares two values.

- **Inputs**: `A`, `B`
- **Outputs**: `Result`
- **Properties**: Operator (==, !=, >, <, >=, <=, contains, startswith, endswith), data type

#### Value Selector

Routes one of two inputs based on a boolean.

- **Inputs**: `Select`, `Input 1`, `Input 2`
- **Outputs**: `Value`

#### Multiplexer

Routes one of many inputs based on index.

- **Inputs**: `Index`, `Value-1` through `Value-N`
- **Outputs**: `Value`, `Selected Index`

#### Move

Passes value through when enabled.

- **Inputs**: `Enable`, `Value`
- **Outputs**: `Value`

#### Math

Basic arithmetic: Add, Subtract, Multiply, Divide.

- **Inputs**: `Enable`, `A`, `B`
- **Outputs**: `Done`, `Result`

#### Advanced Math

Functions: ABS, SQRT, SIN, COS, TAN, SIGN.

- **Inputs**: `Enable`, `Value`
- **Outputs**: `Done`, `Value`

#### Flip-Flop

Stores one bit with Set/Reset control.

- **Inputs**: `Set`, `Reset`
- **Outputs**: `Value`
- **Properties**: RS or SR mode

#### Value Latch

Stores a value until reset.

- **Inputs**: `Value`, `Set`, `Reset`
- **Outputs**: `Value`

#### Edge Value Mux

On trigger, outputs a pulse and the corresponding value.

- **Inputs**: `Trigger 1-4`, `Value 1-4`
- **Outputs**: `Pulse`, `Value`

#### Conversion Helpers

- **Int to Bool**: Non-zero becomes true
- **Bool to Int**: True becomes 1, false becomes 0

---

### Data & Transformation

#### Queue FIFO

First-In First-Out buffer.

- **Inputs**: `Value`, `Add`, `Pop`, `Reset`
- **Outputs**: `Value`, `Has`, `Count`

#### Queue LIFO

Last-In First-Out buffer (stack).

- Same inputs/outputs as FIFO

#### Data Table

Eight configurable registers.

- **Outputs**: Eight values you can rename

#### JSON Key Extractor

Extracts a value from JSON using a key path.

- **Inputs**: `Extract`, `String`, `Reset`
- **Outputs**: `Value`, `Found`
- **Properties**: Key path (supports dot notation)

#### JSON To Node

Splits a JSON object into individual outputs.

- **Inputs**: `Process`, `Reset`, `JSON`
- **Outputs**: `Processed`, `Error`, plus one output per key

#### JSON Array Selector

Picks an element from a JSON array by index.

- **Inputs**: `Process`, `JSON Array`
- **Outputs**: `Processed`, `Found`, `Error`, `Item JSON`

#### JSON Viewer

Displays formatted JSON.

- **Inputs**: `JSON`
- **Properties**: Indent, height, color theme

#### SQL Nodes

- **SQL Connection**: Connect to MySQL, Postgres, SQL Server, or SQLite
- **SQL Query**: Run SELECT and get results as JSON
- **SQL Insert/Update/Delete**: Modify data

All SQL nodes share a connection key to keep the database connection alive.

---

### Networking

#### HTTP Request

Makes HTTP requests (GET, POST, etc.).

- **Inputs**: `Execute`, `Reset`, `Body`
- **Outputs**: `Success`, `Status`, `Body`, `Error`, `Executed`, `Image ID`
- **Properties**: URL, method, headers, body, authentication, TLS options

#### Webhook Listener

Runs an HTTP endpoint to receive external requests.

- **Inputs**: `Enable`, `Custom Response`, `Response Body`, `Response Code`
- **Outputs**: `New Request`, `Body`, `Error`, plus query parameter outputs
- **Properties**: Port, path, authentication, rate limit, query parameters

#### TCP Server

Listens on a TCP port.

- **Outputs**: `New Message`, `Message`
- **Properties**: Port, rate limit

#### TCP Message Sender

Sends TCP messages.

- **Inputs**: `Send`, `Message`
- **Outputs**: `Sent`
- **Properties**: Host, port

---

### Visualization & Debugging

#### Bool Debug Output

Shows boolean state with customizable icons and colors.

- **Inputs**: `Value`
- **Properties**: Icons, colors, labels for true/false states

#### String Debug Output

Shows the current string value.

- **Inputs**: `Value`

#### Value Debug Output

Shows all input types in a table.

- **Inputs**: `Value - Bool`, `Value - Int`, `Value - Float`, `Value - String`

#### Logging

Shows runtime log messages with timestamps.

- **Inputs**: None (automatically receives log messages)

#### Oscilloscope

Plots up to five signals on a scrolling chart.

- **Inputs**: Five signal inputs
- **Properties**: Colors, min/max ranges

#### Speedometer

Gauge display for numeric values.

- **Inputs**: `Value`
- **Properties**: Min, max, segments, colors

#### Text

Static label on the canvas.

- **Properties**: Text content, size, color

---

### Advanced & Utility

#### Lua Script

Runs custom Lua code.

- **Inputs**: `Execute`, plus additional inputs you can use in the script
- **Outputs**: Error output, plus additional outputs you can set in the script

Built-in helpers: `json_encode`, `json_decode`, `time()`, `date()`

#### Teleport

Mirrors signals without drawing long wires.

When adding, select which outputs to mirror. The teleport node then shows those values.

#### Composite Nodes

Reusable node groups you create from selected nodes.

- Create by selecting connected nodes and clicking "Create Composite"
- Manage via the Composites button in the action bar
- Import/export as JSON files
