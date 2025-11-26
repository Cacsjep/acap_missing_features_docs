# Flow

Design automation visually by dragging nodes onto a canvas and wiring them together. Flows can listen to Axis events, apply logic and timing, call HTTP endpoints, and debug live data without coding.

---

#### Open the Flow Builder

- **Launch the app UI** and pick **Flow** from the feature list.
- The last saved flow loads automatically; if none is stored, a short intro dialog appears.

---

#### Floating Action Bar (left, draggable)

- **Start / Stop Execution:** Switches between edit mode and live mode. Live mode locks editing, animates edges, and runs the graph. Stopping saves the graph.
- **Add Node:** Opens the catalog. Drag a template to the canvas. Axis nodes open an extra dialog to pick ports or events before placement.
- **IO Table:** Opens a live table listing every node with its inputs/outputs and current values.
- **Fit to View:** Reframes the canvas around all nodes.
- **Export / Import:** Download or load a graph JSON. Import is disabled while the flow is running.
- **Introduction:** Reopen the short walkthrough (video steps).

**Video: Floating action bar**
<video controls muted loop playsinline src="bar.mp4" style="max-width: 640px; width: 100%; border-radius: 6px;"></video>



---

#### Canvas and Wiring

- **Pan/Zoom:** Drag the background to pan; use the mouse wheel to zoom.
- **Select/Delete:** Hold **Shift** and drag to multi-select; press **Delete** to remove selected nodes/edges.
- **Edges:** Hover to add bend points (yellow). Right-click a bend point to remove. Double-click an edge or click its small **x** to delete.
- **Handles:** Outputs on the right, inputs on the left. Shapes indicate type: **round** = bool, **square** = int/float, **diamond** = string. In live mode booleans glow green/red; int uses magenta, float uses purple, string uses cyan.
- **Edge colors:** Mirror the source output in live mode; turn gray in edit mode.

**Video: Adding nodes**
<video controls muted loop playsinline src="addnode.mp4" style="max-width: 640px; width: 100%; border-radius: 6px;"></video>

**Video: Connect and disconnect**
<video controls muted loop playsinline src="connect.mp4" style="max-width: 640px; width: 100%; border-radius: 6px; margin-top: 6px;"></video>

**Video: Rectangle selection and delete**
<video controls muted loop playsinline src="delete.mp4" style="max-width: 640px; width: 100%; border-radius: 6px; margin-top: 6px;"></video>

---

#### Node Properties and Actions

- Hover a node to show a toolbar for **copy**, **delete**, and **disconnect all edges** (disabled in live mode).
- Click a node in edit mode to open the right-side drawer:
  - Rename **Title/Sub Title**, change **Header Color**, and rename inputs/outputs.
  - Nodes with missing required data show an orange dotted border until fixed.
- A small badge at the top-right shows the last execution time while running; a warning banner appears if the websocket reconnects.

**Video: Node properties**
<video controls muted loop playsinline src="props.mp4" style="max-width: 640px; width: 100%; border-radius: 6px;"></video>

---

#### Node Reference

Use **Add Node** to drag any template. Each entry lists what it does, its ports, and what you can configure.

##### Axis and device

- **Axis Device Log**  
    - What it does: Write a message to the Axis device syslog.  
    - Inputs: `Write` (bool), `Message` (string).

- **Axis Metadata Event (Subscribe)**  
    - What it does: Subscribe to Axis metadata events and expose fields as outputs.  
    - Inputs: `Enable`.  
    - Outputs: `Connected`, `Error`, `New Event`, plus one output per Axis field; override inputs let you simulate values.

- **Axis Metadata Event (Create)**  
    - What it does: Create a custom stateless Axis event on this device (visible in Axis rules while running).  
    - Inputs: `Send` plus user-added fields marked as **Trigger (Source)** or **Data**.  
    - Outputs: `Error`.  
    - Properties: Event name; add/remove fields and choose their type/direction.
<video controls muted loop playsinline src="axiscreate.mp4" style="max-width: 640px; width: 100%; border-radius: 6px;"></video>

!!! warning
    Axis Metadata Event (Create) nodes are only visible in the Axis event rule list while the flow is running.

- **Axis Live Stream**  
    - What it does: RTSP video preview from an Axis camera (live mode only).  
    - Properties: IP, port, username, password, channel. Shows connecting/error overlays and retries every 5 s on failure.

- **Axis Outputs**  
    - What it does: Toggle Axis output ports via VAPIX.  
    - Inputs: `EN` plus one input per discovered port.  
    - Outputs: `Success`, `Status`, `Error`.  
    - Tip: Recreate the node if device credentials or port mapping change.
<video controls muted loop playsinline src="axissub.mp4" style="max-width: 640px; width: 100%; border-radius: 6px;"></video>

##### Inputs and manual controls

- **Manual Input** - Constant output; pick data type and value.  
- **Enable Input** - Always outputs `true`.  
- **Button** - Outputs `true` while pressed, `false` on release (live mode).

**Video: Manual inputs and buttons**
<video controls muted loop playsinline src="manual.mp4" style="max-width: 640px; width: 100%; border-radius: 6px;"></video>

**Video: Handle shapes and colors**
<video controls muted loop playsinline src="io.mp4" style="max-width: 640px; width: 100%; border-radius: 6px; margin-top: 6px;"></video>

##### Timing and pulses

- **Debounce** - Boolean filter; output changes only after the input is stable for the set time. Properties: unit (ms/s/m), amount.  
- **Edge Detection** - Rising or falling edge detector; outputs a pulse.  
- **Pulse Buffer with Release** - Buffers pulses until `Release` goes true. Inputs: `Set`, `Release`, `Clear`. Property: buffer size.  
- **Timer on Delay (TON)** - `Active` turns true after `Enable` stays true for the configured time; outputs elapsed time.  
- **Timer off Delay (TOF)** - `Active` turns true immediately on rising edge and stays true for the set time after `Enable` goes false.  
- **Pulse Timer (TP)** - Fixed-length pulse on each rising edge of `Enable`; outputs elapsed time.  
- **Pulse Generator** - Periodic single-cycle pulse while `Enable` is true.  
- **Step Sequencer** - Advances one step per `Step` pulse; exactly one output is true. Property: steps (2-16).  
- **Timed Sequenced** - Runs timers in order while enabled, then wraps back to the first timer. 
- **Counter** - Counts up/down on pulses; `Reset` clears to zero.

##### Logic, selection, and math

- **Boolean gates (AND, NAND, OR, NOR, XOR, XNOR, NOT)** - Standard two-input gates (NOT is single-input).  
- **Compare** - Compares `A` and `B` (`==, !=, >, <, >=, <=, contains, startswith, endswith`). Properties: operator; data type (int/float/string); inputs reset to the chosen type.  
- **Value Selector** - Switch between two inputs with `Select` (false = input 1, true = input 2). Property: shared data type.  
- **Multiplexer** - Route one of many value inputs to output based on numeric `Index`. Properties: data type; add/remove value inputs; output also reports selected index.  
- **Move** - Pass `Value` to output when `Enable` is true.  
- **Math** - Add/Subtract/Multiply/Divide `A` and `B` when `Enable` is true. Outputs: `Done`, `Result`. Property: operation.  
- **Advanced Math** - ABS, SQRT, SIN, COS, TAN, SIGN on `Value` when `Enable` is true. Outputs: `Done`, `Value`. Property: operation.  
- **Flip-Flop** - Set/Reset latch. Properties: mode `RS` (reset-dominant) or `SR` (set-dominant); input labels update automatically.  
- **Value Latch** - Stores the last value when `Set` is true; holds until `Reset`. Property: data type syncs input/output.

##### Data, buffers, routing

- **Queue First-In First-Out / Queue Last-In First-Out** - Push while `Enable` is true; `Pop` returns the next value; `Reset` clears. Outputs: `Value`, `Has`, `Count`. Properties: max size, data type.  
- **Data Table** - Multi-output register block; add/remove outputs and set data types.  
- **Routing helpers** - Combine Value Selector, Multiplexer, and Move to branch, gate, or forward values unchanged.

##### Networking and parsing

- **HTTP Request** - Fires when `Enable` is true; `Reset` clears state. Outputs: `Success`, `Status`, `Body`, `Error`. Properties: URL, method, body type (none/form/JSON/raw/XML), auth (none/basic/digest), headers, params, timeout, skip TLS verify.  
- **JSON Key Extractor** - On `Extract`, pulls a key (dot notation allowed) from a JSON string; `Reset` clears. Outputs: `Value`, `Found`. Properties: key and output type.

##### Debug and visualization

- **Bool Debug Output** - Large icon/label that follows the boolean state. Properties: icons, colors, labels for true/false.  
- **String Debug Output** - Shows latest string input.  
- **Value Debug Output** - Table of all input states (bool/int/float/string).  
- **Oscilloscope** - Plots up to five signals. Properties: line colors, per-input data type, min/max ranges for numeric inputs.  
- **Logging** - Live log viewer (level, time, node id/name, message) from the flow runtime.  
- **Text** - Static label. Properties: text and size.  
- **Axis Live Stream** - Also handy for visual debugging of camera feeds.
- **Speedometer** - The Speedometer visualizes a numeric input value using a gauge-style dial.

---

#### Tips

- If a node shows an orange dotted border, open its properties and complete the required fields (URL, event name, etc.).
- Keep a **Logging** node in your flow to see backend messages when testing HTTP calls or Axis events.
