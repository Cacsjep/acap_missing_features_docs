# Network & Communication Nodes

Nodes for industrial protocols, TCP/IP communication, and media streaming.

---

## Modbus TCP

Modbus TCP is an industrial protocol widely used in building automation, PLCs, and SCADA systems. The **Modbus** node provides a unified interface to connect to Modbus TCP servers and read/write various data types.

### Modbus Node

The **Modbus** node establishes a connection to a Modbus TCP server and manages multiple configurable items (coils, registers) in a single node.

**Category**: Network, Modbus

#### Functionality

- Connects to a Modbus TCP server with automatic reconnection (5-second retry on connection loss)
- Configurable items define what data to read/write (coils, integers, floats, strings)
- Each item gets its own read trigger, write trigger, value input, and value output
- Built-in keepalive mechanism reads coil 0 every 2 seconds to prevent idle timeouts
- Uses Big-Endian byte order (IEEE 754 standard) for floats and strings

#### Base Inputs

| Name | Type | Description |
|------|------|-------------|
| Enable | Boolean | TRUE connects to the server; FALSE disconnects |
| Read All | Boolean | Rising edge triggers read of all configured items |
| Write All | Boolean | Rising edge triggers write of all items using their Value inputs |

#### Base Outputs

| Name | Type | Description |
|------|------|-------------|
| Connected | Boolean | TRUE while connected to the Modbus server |
| Op Complete | Boolean | Pulses TRUE for one cycle when any read/write operation completes |

#### Per-Item I/O

For each configured item, the node generates:

| Name | Type | Description |
|------|------|-------------|
| {name} Read (input) | Boolean | Rising edge triggers read of this item |
| {name} Write (input) | Boolean | Rising edge triggers write of this item |
| {name} Value (input) | Configurable | Value to write when Write is triggered |
| {name} (output) | Configurable | Current value read from this item |

#### Properties

- **Host**: IP address or hostname of the Modbus server (e.g., `192.168.1.100`)
- **Port**: TCP port (default: `502`)
- **Unit ID**: Modbus unit/slave identifier (0-255, default: `1`)

#### Item Configuration

Click **Add Item** to configure Modbus data points:

| Property | Description |
|----------|-------------|
| Name | Unique identifier for this item (creates I/O labels) |
| Type | Data type: `coil`, `int`, `float`, or `string` |
| Address | Register/coil address (coils use 1-based addressing, registers use 0-based) |
| Length | For strings only: maximum character length |

After adding or removing items, click **Sync I/O** to update the node's inputs and outputs.

#### Item Types

| Type | Description | Registers Used |
|------|-------------|----------------|
| coil | Boolean value (ON/OFF) | 1 coil |
| int | 16-bit signed integer (-32768 to 32767) | 1 register |
| float | 32-bit IEEE 754 floating-point | 2 registers |
| string | ASCII text (2 characters per register) | length / 2 registers |

---

## Modbus Example

A typical Modbus setup:

1. Add a **Modbus** node and configure the server Host, Port, and Unit ID
2. Add items for the data points you need (coils, registers)
3. Click **Sync I/O** to generate the inputs and outputs
4. Wire an **Enable** signal or constant TRUE to connect
5. Use the per-item Read/Write inputs or Read All/Write All for batch operations
6. Monitor **Op Complete** to trigger downstream logic when operations finish

!!! tip
    Use **Read All** with a timer to poll all items periodically. Use per-item **Read** triggers for on-demand reads.

!!! warning
    Ensure the Modbus server is accessible from the camera network. Firewall rules may need adjustment for port 502.

---

## OPC UA

OPC UA (Open Platform Communications Unified Architecture) is a machine-to-machine communication protocol for industrial automation. The Flow provides nodes to connect to OPC UA servers and read/write variables.

### OPC UA Client

The **OPC UA Client** node establishes and manages a connection to an OPC UA server.

**Category**: Network, OPC UA

#### Functionality

Creates a persistent connection to an OPC UA server. Other OPC UA nodes use the `Conn Key` output to share this connection.

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Connect | Boolean | Rising edge opens the connection; falling edge closes it |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Connected | Boolean | True while connection is active |
| Conn Key | String | Unique key to pass to read/write nodes |
| Error | String | Connection error message (if any) |

#### Properties

- **Endpoint URL**: OPC UA server endpoint (e.g., `opc.tcp://192.168.1.100:4840`)
- **Security Mode**: None, Sign, or SignAndEncrypt
- **Security Policy**: None, Basic256Sha256, etc.
- **Username**: Optional authentication username
- **Password**: Optional authentication password

!!! tip
    Keep one OPC UA Client node and wire its `Conn Key` to multiple read/write nodes. This shares the connection efficiently.

---

### OPC UA Read

Reads a value from an OPC UA node variable.

**Category**: Network, OPC UA

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from OPC UA Client |
| Read | Boolean | Rising edge triggers the read |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Read | Boolean | Pulses true when read completes |
| Value | Configurable | The variable value (type selectable) |
| Error | String | Error message if read fails |

#### Properties

- **Node ID**: The OPC UA node identifier (e.g., `ns=1;s=Temperature`)
- **Output Type**: Select the data type (Boolean, Integer, Float, String)

---

### OPC UA Write

Writes a value to an OPC UA node variable.

**Category**: Network, OPC UA

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from OPC UA Client |
| Write | Boolean | Rising edge triggers the write |
| Value | Configurable | The value to write (type selectable) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true when write completes |
| Error | String | Error message if write fails |

#### Properties

- **Node ID**: The OPC UA node identifier (e.g., `ns=1;s=SetPoint`)
- **Input Type**: Select the data type (Boolean, Integer, Float, String)

---

### OPC UA Example

A typical OPC UA setup:

1. Add an **OPC UA Client** node and configure the server endpoint URL
2. Wire an **Enable** node or trigger to the `Connect` input
3. Add **OPC UA Read/Write** nodes and connect their `Conn Key` input to the client's output
4. Use timers or triggers to control when reads/writes occur

!!! warning
    Ensure the OPC UA server is accessible from the camera network. Firewall rules may need adjustment for port 4840 (default OPC UA port).

---

## Media Streaming

### AXIS Live Stream

The **AXIS Live Stream** node displays live video from an Axis device using WebCodecs for H.264 decoding.

**Category**: Axis, Network

#### Functionality

Enables real-time video streaming with low latency. Uses WebCodecs instead of WebRTC, eliminating NAT traversal issues - works reliably behind firewalls without STUN/TURN servers.

#### Properties

- **Device**: Select the Axis device from Device Management
- **Resolution**: Optional resolution override

#### Stream Statistics

When streaming, view real-time stats including:

- Resolution and framerate
- Bitrate
- Decode queue size

!!! tip
    WebCodecs provides reliable streaming without NAT/firewall configuration. The stream connects directly via WebSocket.

---

### RTMP Stream

The **RTMP Stream** node publishes video to RTMP media servers or streaming platforms.

**Category**: Axis, Network

#### Functionality

Streams the camera feed to YouTube Live, Twitch, or custom RTMP servers.

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Publish | Boolean | True starts streaming; false stops |

#### Properties

- **Device**: Select the Axis device
- **RTMP URL**: Server URL (e.g., `rtmp://a.rtmp.youtube.com/live2`)
- **Stream Key**: Authentication key for the platform
- **Resolution**: Video resolution to stream

!!! warning
    Streaming uses significant bandwidth. Ensure adequate upload capacity.

---

## TCP Communication

### TCP Server

Listens on a TCP port and outputs received messages.

**Category**: Input, Network

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| New Message | Boolean | Pulses true when data arrives |
| Message | String | The received payload |

#### Properties

- **Port**: TCP port to listen on (1-65535)
- **Rate Limit**: Maximum messages per second

---

### TCP Message Sender

Sends string payloads over TCP to a remote host.

**Category**: Action, Network

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Send | Boolean | Rising edge sends the message |
| Message | String | Payload to transmit |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Sent | Boolean | Pulses true on success |

#### Properties

- **Host**: Destination IP or hostname
- **Port**: Destination port
- **Timeout**: Connection timeout in seconds

---

## HTTP & Webhooks

### Webhook Listener

Runs an HTTP/HTTPS endpoint to receive external requests.

**Category**: Input, Network

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Custom Response | Boolean | Enable custom response body |
| Response Body | String | Custom JSON response |
| Response Code | Integer | HTTP status code to return |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| New Request | Boolean | Pulses true on each request |
| Body | String | Request body content |
| Query params | String | One output per configured parameter |

#### Properties

- **Port**: HTTP or HTTPS port
- **Path**: URL path (e.g., `/webhook`)
- **Auth Type**: None, Basic, or Digest
- **Rate Limit**: Requests per second (1-15)
- **Query Parameters**: Define expected URL parameters

The webhook always responds with:
```json
{"success": true, "message": "OK"}
```

Unless custom response is enabled.

!!! tip
    Use query parameters to pass identifiers like `device_id` or `event_type` without parsing the body.
