# Network & Communication Nodes

Nodes for industrial protocols, TCP/IP communication, and media streaming.

---

## Modbus TCP

Modbus TCP is an industrial protocol widely used in building automation, PLCs, and SCADA systems. The Flow provides a complete set of nodes to connect to Modbus TCP servers and read/write various data types.

### Modbus TCP Client

The **Modbus TCP Client** node establishes and manages a connection to a Modbus TCP server.

**Category**: Network, Modbus

#### Functionality

Creates a persistent connection to a Modbus server. Other Modbus nodes use the `Conn Key` output to share this connection.

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Connect | Boolean | Rising edge opens the connection; falling edge closes it |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Connected | Boolean | True while connection is active |
| Conn Key | String | Unique key to pass to read/write nodes |

#### Properties

- **Host**: IP address or hostname of the Modbus server (e.g., `192.168.1.100`)
- **Port**: TCP port (default: `502`)
- **Slave ID**: Modbus slave/unit ID (default: `1`)

!!! tip
    Keep one TCP Client node and wire its `Conn Key` to multiple read/write nodes. This shares the connection efficiently.

---

### Modbus Write Coil

Writes a boolean value to a single coil register.

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Write | Boolean | Rising edge triggers the write |
| Value | Boolean | The value to write (ON/OFF) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true when write completes |

#### Properties

- **Address**: Coil address (0-65535)

---

### Modbus Read Coil

Reads a boolean value from a single coil register.

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Read | Boolean | Rising edge triggers the read |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Read | Boolean | Pulses true when read completes |
| Value | Boolean | The coil state |

#### Properties

- **Address**: Coil address (0-65535)

---

### Modbus Write Int

Writes a 16-bit integer value to a holding register.

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Write | Boolean | Rising edge triggers the write |
| Value | Integer | The value to write (-32768 to 32767) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true when write completes |

#### Properties

- **Address**: Holding register address (0-65535)

---

### Modbus Read Int

Reads a 16-bit integer value from a holding register.

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Read | Boolean | Rising edge triggers the read |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Read | Boolean | Pulses true when read completes |
| Value | Integer | The register value |

#### Properties

- **Address**: Holding register address (0-65535)

---

### Modbus Write Float

Writes a 32-bit IEEE 754 floating-point value to two consecutive holding registers.

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Write | Boolean | Rising edge triggers the write |
| Value | Float | The floating-point value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true when write completes |

#### Properties

- **Address**: Starting holding register address (uses 2 registers)

---

### Modbus Read Float

Reads a 32-bit IEEE 754 floating-point value from two consecutive holding registers.

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Read | Boolean | Rising edge triggers the read |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Read | Boolean | Pulses true when read completes |
| Value | Float | The floating-point value |

#### Properties

- **Address**: Starting holding register address (uses 2 registers)

---

### Modbus Write String

Writes a string to multiple holding registers (2 ASCII characters per register).

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Write | Boolean | Rising edge triggers the write |
| Value | String | The text to write |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true when write completes |

#### Properties

- **Address**: Starting holding register address
- **Length**: Maximum string length (number of registers = length / 2)

---

### Modbus Read String

Reads a string from multiple holding registers (2 ASCII characters per register).

**Category**: Network, Modbus

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from Modbus TCP Client |
| Read | Boolean | Rising edge triggers the read |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Read | Boolean | Pulses true when read completes |
| Value | String | The retrieved text |

#### Properties

- **Address**: Starting holding register address
- **Length**: Number of characters to read

---

## Modbus Example

A typical Modbus setup:

1. Add a **Modbus TCP Client** node and configure the server IP and port
2. Wire an **Enable** node or trigger to the `Connect` input
3. Add **Modbus Read/Write** nodes and connect their `Conn Key` input to the client's output
4. Use timers or triggers to control when reads/writes occur

!!! warning
    Ensure the Modbus server is accessible from the camera network. Firewall rules may need adjustment.

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

### WebRTC Stream

The **WebRTC Stream** node streams video from an Axis device over WebRTC protocol.

**Category**: Axis, Network

#### Functionality

Enables real-time video streaming with low latency. Supports both LAN connections and TURN/STUN for NAT traversal.

#### Properties

- **Device**: Select the Axis device from Device Management
- **Resolution**: Optional resolution override
- **TURN/STUN**: Configure relay servers for remote access

!!! tip
    WebRTC provides lower latency than RTSP for interactive applications.

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
