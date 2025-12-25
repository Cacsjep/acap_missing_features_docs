# Storage & SD Card Nodes

Nodes for file operations on the Axis SD card and image caching.

---

## SD Card Overview

The Axis camera's SD card can be used for persistent storage of logs, configuration, and captured images. These nodes provide full file system access within the SD card.

!!! warning
    Ensure an SD card is inserted and formatted before using these nodes. Check the camera's Storage settings.

---

## SD Card Status

Reports the current state of the SD card.

**Category**: Axis, SD-Card

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Present | Boolean | True if card is inserted |
| Writable | Boolean | True if card can be written to |
| Full | Boolean | True if card is at capacity |
| Setup | Boolean | True if card is being configured |
| Exiting | Boolean | True if card is being ejected |

!!! tip
    Use this node to verify the SD card is available before attempting file operations.

---

## SD Card File Exists

Checks whether a file exists at the specified path.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the check |
| Path | String | Relative path on SD card (e.g., `logs/data.csv`) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Exists | Boolean | True if file exists |

---

## SD Card Write File

Creates or overwrites a file with the provided content.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the write |
| Path | String | Relative path on SD card |
| Data | String | Content to write |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true on success |

!!! warning
    This overwrites existing files completely. Use Append File for logging.

---

## SD Card Append File

Appends data to an existing file or creates a new one.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the append |
| Path | String | Relative path on SD card |
| Data | String | Content to append |
| New Line After | Boolean | Add newline after data |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Written | Boolean | Pulses true on success |

#### Example: CSV Logging

1. Use **CSV Row Builder** to format data
2. Connect to **SD Card Append File** with `New Line After` = true
3. Set path to `logs/events.csv`

---

## SD Card Read File

Reads the contents of a file.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the read |
| Path | String | Relative path on SD card |
| Reset | Boolean | Clears cached content |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Data | String | File contents |
| File Not Exists | Boolean | True if file not found |

---

## SD Card Remove File

Deletes a file from the SD card.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers deletion |
| Path | String | Relative path to delete |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Removed | Boolean | Pulses true on success |

---

## SD Card List Files

Returns a list of all files on the SD card.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the scan |
| Reset | Boolean | Clears cached list |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Data | String | JSON array of file paths |
| Count | Integer | Number of files |

#### Example Output

```json
["logs/events.csv", "images/capture001.jpg", "config.json"]
```

---

## SD Card Usage

Reports storage consumption.

**Category**: Axis, SD-Card

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the query |
| Reset | Boolean | Clears cached value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Used GB | Integer | Gigabytes used on card |

---

## Image Cache

The image cache stores captured images in memory for quick access without writing to disk. This is useful for passing images between nodes (e.g., HTTP uploads, viewers) without large data transfers.

### Image Cache Info

Reports statistics about the in-memory image cache.

**Category**: Image, Cache

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Has Images | Boolean | True if cache contains images |
| Count | Integer | Number of cached images |
| Total KB | Float | Total size in kilobytes |

!!! note
    The cache holds up to 10 images or 30 MB total. Oldest images are evicted when full.

---

### Image Cache Delete

Clears images from the cache.

**Category**: Image, Cache

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Clear | Boolean | Rising edge clears the cache |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Cleared | Boolean | Pulses true when complete |
| Count | Integer | Number of images that were cleared |

---

## Image Nodes

### Image Snapshot

Captures a JPEG snapshot from an Axis device.

**Category**: Axis, Image

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Capture | Boolean | Rising edge triggers capture |
| Reset | Boolean | Clears the image reference |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Success | Boolean | True if capture succeeded |
| Image ID | String | Reference ID for the cached image |
| Error | String | Error message if failed |
| Executed | Boolean | Pulses true when complete |

#### Properties

- **Device**: Select from Device Management
- **Resolution**: Optional resolution override

!!! tip
    Pass the `Image ID` to JPEG Viewer or HTTP Request nodes instead of transferring raw image bytes.

---

### JPEG Viewer

Displays a cached image by its ID.

**Category**: Axis, Image

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Image ID | String | Reference from Image Snapshot, HTTP, or Vaxtor |

#### Properties

- **Width/Height**: Display dimensions
- **Border**: Show frame border

The viewer automatically fetches the image from the server-side cache, keeping websocket traffic minimal.

---

## Syslog

Writes messages to the Axis device system log.

**Category**: Axis, Debug

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Write | Boolean | Rising edge writes the message |
| Message | String | Text to log |

!!! tip
    Use Syslog to record important events for troubleshooting via the camera's log viewer.
