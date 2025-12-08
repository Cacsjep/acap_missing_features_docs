# Advanced Flow Nodes

Nodes for scripting, wireless connections, and dynamic data structures.

## Lua Script
The **Lua Script** node executes custom Lua code.

**Category**: Advanced
**Type**: `LuaNode`

### Functionality
Runs a Lua script when the **Execute** input is triggered. It provides a sandboxed environment to interact with inputs and write to outputs.

### Inputs
- **Execute** (Boolean): Trigger signal (Rising edge).
- **I.xx**: Dynamic data inputs available in the script.

### Outputs
- **O.01 (Error)**: Reserved for error messages.
- **O.xx**: Dynamic outputs writable from the script.

### Scripting API
The script has access to global `inputs` and `outputs` tables.

#### Variables
- `inputs["I.01"]`: Read input value.
- `outputs["O.02"] = true`: Write output value.

#### Standard Libraries
- `math` (abs, max, min, sqrt, etc.)
- `string` (len, sub, find, match, etc.)
- `table` (insert, remove, sort, etc.)

#### Helpers
- `time()`: Current Unix timestamp.
- `date(format)`: Formatted date string (default: `%Y-%m-%d %H:%M:%S`).
- `json_encode(val)`: Returns JSON string.
- `json_decode(str)`: Returns Lua table.

> **Note**: OS and File System access is blocked for security.

---

## Teleport
The **Teleport** node transfers a signal wirelessly to another location on the canvas.

**Category**: Advanced
**Type**: `TeleportNode`

### Functionality
Works as a "portal". You select a source node and its output, and the Teleport node mirrors that value. This eliminates long, messy connection wires.

### Configuration
1.  Add the node.
2.  A dialog will prompt you to select the **Source Node** and the specific **Output** to teleport.

---

## JSON To Node
The **JSON To Node** node splits a JSON object into individual outputs.

**Category**: Advanced
**Type**: `JsonToNodeNode`

### Functionality
Takes a JSON object and creates a dedicated output terminal for each top-level key found in the sample JSON.

### Inputs
- **Process** (Boolean): Triggers the parsing.
- **Reset** (Boolean): Clears all outputs.
- **JSON** (String): The JSON payload.

### Outputs
- **Processed** (Boolean): High after successful parse.
- **Error** (String): Error message if parsing fails.
- **[Dynamic Keys]**: One output per key (e.g., `data`, `status`).

### Configuration
- **Sample JSON**: Paste a sample JSON structure in the node properties to generate the corresponding output ports.
