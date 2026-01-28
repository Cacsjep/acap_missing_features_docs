# Device Integration Nodes

Nodes for interacting with physical Axis devices, streams, and system events.

## Axis RTSP Stream
The **Axis RTSP Stream** node displays a live video stream from an Axis device and can overlay metadata.

**Category**: Device
**Type**: `AxisRtspStreamNode`

### Functionality
Connects to a device's RTSP stream. Supports visualizing region-of-interest (ROI) and object detection metadata overlays.

### Inputs
- **ROI** (JSON): Region of Interest polygon points (if overlay enabled).
- **Observations** (JSON): Object detection data to draw bounding boxes.
- **Tripwire** (JSON): Tripwire line definition.

### Configuration
1.  **Device**: Select the target Axis device from the dropdown property.
2.  The node will attempt to connect and display the stream automatically when the flow is "Live".

---

## Axis Output
The **Axis Output** node controls physical I/O ports on the device (e.g., relays, digital outs).

**Category**: Device
**Type**: `AxisOutputNode`

### Functionality
Sets the state of a physical port based on the input signal.

### Inputs
- **In 1, In 2, ...**: Boolean inputs corresponding to the device's ports.

### Configuration
-  **Port Mapping**: Ports are typically mapped when adding the node or via the device selection dialog.

---

## Metadata Event Create
The **Metadata Event Create** node defines a custom event that can be emitted by the device.

**Category**: Device
**Type**: `MetaDataEventCreateNode`

### Functionality
Constructs a structured event payload. This can be used to trigger rules on the device or be consumed by other services.

### Properties
- **Event Name**: Unique identifier for the event.
- **Event Type**:
  - **Stateless** (Trigger): Pulse-like event (e.g., "Motion Detected").
  - **Stateful**: Persistent state (e.g., "Door Open").

### Inputs
- **Custom Data fields**: Add fields via the "+" button in properties. You can define them as:
  - **Source**: Origin of the event.
  - **Data**: Payload value.

---

## Metadata Event Subscribe
The **Metadata Event Subscribe** node listens for system or custom events.

**Category**: Device
**Type**: `MetaDataEventSubscribeNode`

### Functionality
Fires outputs when a specific event occurs on the device (e.g., specific MQTT topic or internal rule).

### Configuration
- **Setup**: When adding this node, a dialog typically appears to select the specific event topic you wish to subscribe to.
- **Outputs**: Dynamic outputs are created based on the event's data fields.

---

## Analytics Metadata

The **Analytics Metadata** node receives raw analytics metadata from the device's built-in object detection (AXIS Object Analytics).

**Category**: Device
**Type**: `AnalyticsMetadataNode`

### Functionality

Streams real-time object detection data from the device's analytics engine. Supports two modes for different use cases.

### Modes

| Mode | Description |
|------|-------------|
| **Scene Description** | Real-time frames with current observations (objects in frame) |
| **Consolidated Track** | Complete track data after an object leaves the scene |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| New Data | Boolean | Pulses true when new metadata arrives |
| JSON | String | Raw JSON containing observations or track data |
| Error | String | Error message if streaming fails |

### Properties

- **Mode**: Select Scene Description or Consolidated Track
- **Framerate**: How often to poll for new data (frames per second)

### Scene Description JSON Example

```json
{
  "observations": [
    {
      "id": "track_123",
      "type": "Human",
      "boundingBox": {"x": 0.2, "y": 0.3, "width": 0.1, "height": 0.3},
      "confidence": 0.95
    }
  ]
}
```

!!! tip
    Use Scene Description for real-time monitoring and triggering. Use Consolidated Track for logging complete object paths after they leave the frame.

---

## License Plate List
The **License Plate List** node checks a plate string against a configured local database.

**Category**: Device / ANPR
**Type**: `LicensePlateListNode`

### Functionality
Verifies if a license plate exists in the internal list and checks its validity (expiration, allowed time windows).

### Inputs
- **Process** (Boolean): Triggers the lookup.
- **Plate** (String): The text to check.
- **Reset** (Boolean): Clears outputs.

### Outputs
- **Matched Plate** (String): The best match found.
- **In List** (Boolean): True if found.
- **Not In List** (Boolean): True if not found.
- **Expired** (Boolean): True if found but license is expired.
- **Not Valid Yet** (Boolean): True if valid from current time > now.
- **Tag outputs**: Boolean outputs for each tag (e.g., "Staff", "VIP") associated with the plate.

!!! note "Tag output updates"
    Tag outputs are created when you add the node to your flow based on the tags that exist at that time. If you add new tags to the License Plate List feature later, you must **delete and recreate** the node to get outputs for the new tags. Plate data updates are reflected immediately without recreating the node.

---

## Vaxtor Cloud ANPR
The **Vaxtor Cloud ANPR** node sends an image to the Vaxtor Cloud for recognition.

**Category**: Device / ANPR
**Type**: `VaxtorCloudAnprNode`

### Functionality
Captures an image from the connected device and uploads it to the Vaxtor Cloud API for analysis.

### Inputs
- **Execute** (Boolean): Triggers the capture and upload.
- **Reset** (Boolean): Cancels pending request.

### Outputs
- **Success** (Boolean): API request succeeded.
- **Plate Found** (Boolean): Cloud returned a plate.
- **Number / Country / State** (String): Plate details.
- **Vehicle Make / Model / Color** (String): Vehicle attributes.
- **Status** (Integer): HTTP status code (200=OK) or internal error code.

### Configuration
- **Authentication**: Uses configured client credentials to obtain a token from `auth.vaxtor.cloud`.
