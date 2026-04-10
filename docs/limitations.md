# Limitations & Network Requirements

This page covers important technical limitations and network requirements you should be aware of when deploying Missing Features ACAP.

---

## Live Video Streaming

Several features use live video streaming via **WebCodecs over WebSocket**:

- **Dashboard:** AXIS Live Stream widget
- **Scene Analysis Triggers:** Live preview with overlays
- **Spot Color Detector:** Video feed for color detection regions
- **Flow:** AXIS Live Stream node

### How It Works

Video streaming uses WebCodecs for H.264 decoding over a WebSocket connection:

1. Browser connects to the device via WebSocket (TCP)
2. Device sends H.264 video frames over the WebSocket
3. Browser decodes frames using WebCodecs API

### Network Requirements

| Requirement | Details |
|-------------|---------|
| **Protocol** | TCP (WebSocket over HTTPS) |
| **Ports** | Same port as web UI (Axis HTTP/HTTPS, typically 443) |
| **Direction** | Standard HTTP/WebSocket, routed through the Axis reverse proxy |

### Advantages Over Previous WebRTC Approach

| Aspect | WebCodecs (Current) | WebRTC (Previous) |
|--------|---------------------|-------------------|
| NAT Traversal | Not needed - uses TCP | Required STUN/TURN |
| Firewall | Only the Axis web port needed | Required UDP ports |
| Remote Access | Works if UI works | Often failed remotely |
| Setup | No configuration | Needed relay servers |

### Troubleshooting Video Streams

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| Video not loading | WebSocket blocked | Verify the Axis web interface is reachable from the client |
| Decoder error | Browser compatibility | Use Chrome, Firefox, or Edge |
| Stream freezes | Network interruption | Check network stability |
| No video, no error | Device not streaming | Verify device is accessible |

---

## Browser Requirements

For best compatibility with video streaming features (WebCodecs API):

| Browser | Support |
|---------|---------|
| Chrome 94+ | Fully supported |
| Firefox 130+ | Fully supported |
| Edge 94+ | Fully supported |
| Safari 16.4+ | Supported |

!!! tip
    WebCodecs is a modern browser API for hardware-accelerated video decoding. If video streaming doesn't work, ensure your browser is up to date.

---

## SD Card Requirements

Several features require an SD card to be inserted and formatted:

- **Storage Nodes:** All SD Card file operations
- **Image to SD-Card:** Saving captured images
- **Database (SQLite):** Local database storage

---

## Image Cache Limits

The in-memory image cache has the following constraints:

| Constraint | Value |
|------------|-------|
| Maximum cached images | 10 |
| Maximum cache size | 30 MB |
| Eviction policy | Oldest images removed first |

When the cache is full, the oldest images are automatically evicted to make room for new ones.

---

## Database Constraints

### Connection Settings

| Setting | Default Value |
|---------|---------------|
| Connection timeout | 10 seconds |
| Max retry attempts | 3 |
| Retry delay | 1 second |
| Slow query threshold | 5 seconds |

### Supported databases

SQLite, MySQL, PostgreSQL, SQL Server 

!!! note
    External databases (MySQL, PostgreSQL, SQL Server) require network access from the Axis device to the database server.

---

## Network Timeouts

### Protocol-Specific Timeouts

| Protocol | Timeout/Interval |
|----------|------------------|
| TCP Sender dial | 2 seconds |
| HTTP Request | Configurable (default varies) |
| Webhook Request | 30 seconds |
| Modbus TCP heartbeat | 2 seconds |
| OPC UA heartbeat | 5 seconds |
| OPC UA client close | 2 seconds |
| RTMP dial | 5 seconds |
| RTMP read/write | 2 seconds |

### Default Ports

| Protocol | Default Port |
|----------|--------------|
| Modbus TCP | 502 |
| Valid port range | 1 - 65535 |

---

### Spot Color Detector

| Setting | Value |
|---------|-------|
| Maximum sample points | 8000 |
| Default analysis interval | 500 ms |
| Default framerate | 5 FPS |
| Default hysteresis threshold | 30.0 |

---

## File Download Limits

When downloading files via the SD Card Explorer:

| File Type | Maximum Size |
|-----------|--------------|
| Text files | 25 MB |
| Binary files | 50 MB |

---

## Summary

| Feature | Requires |
|---------|----------|
| Web UI Access | Standard Axis HTTP/HTTPS port (typically 443) |
| Live Video Streaming | Standard Axis HTTP/HTTPS port (same as UI) |
| Remote Access | Reachability to the Axis web interface (direct or VPN) |
| SD Card Features | Inserted and formatted SD card |
| External Databases | Network access to database server |
| Modbus TCP | Port 502 (default) accessible |
