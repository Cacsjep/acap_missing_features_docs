# Limitations & Network Requirements

This page covers important technical limitations and network requirements you should be aware of when deploying Missing Features ACAP.

---

## Dedicated Web Server on Port 8888

Missing Features runs its own **dedicated web server** separate from the standard Axis web interface.

| Aspect | Details |
|--------|---------|
| **Port** | `8888` (HTTPS) |
| **Access URL** | `https://<device-ip>:8888` |
| **Protocol** | HTTPS with self-signed certificate (or custom certificate) |

### Firewall Configuration

Ensure port `8888` is open for TCP traffic:

- **Inbound:** Allow connections to port 8888 on the Axis device
- **From:** Any client that needs to access the Missing Features UI

### Common Issues

| Issue | Solution |
|-------|----------|
| Cannot access UI | Check firewall rules allow port 8888 |
| Connection refused | Verify ACAP is running on the device |
| Certificate warning | Expected with self-signed cert; add exception or upload custom certificate |

---

## Live Video Streaming (UDP Requirements)

Several features use live video streaming that relies on **UDP connections**:

- **Dashboard:** AXIS Live Stream widget
- **Scene Analysis Triggers:** Live preview with overlays
- **Spot Color Detector:** Video feed for color detection regions

### How It Works

The video streaming uses a peer-to-peer connection that requires UDP traffic:

1. The browser and device exchange connection information
2. A direct UDP connection is established for video data
3. Video frames are streamed with low latency

### Network Requirements

| Requirement | Details |
|-------------|---------|
| **Protocol** | UDP (User Datagram Protocol) |
| **Ports** | Dynamic high ports (typically 49152-65535) |
| **Direction** | Bidirectional between browser and device |

### NAT and Router Considerations

When the Axis device is behind a NAT router or firewall, UDP connections may fail. Here are solutions:

#### Option 1: Same Network (Recommended)

Keep the browser/client on the **same local network** as the Axis device. This avoids NAT traversal issues entirely.

#### Option 2: Port Forwarding

If accessing remotely, configure your router to forward:

- Port `8888` TCP (for the web UI)
- UDP port range for video streaming (consult your network administrator)

#### Option 3: VPN

Use a VPN to place the client on the same virtual network as the device. This bypasses NAT issues.

### Troubleshooting Video Streams

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| Video not loading | UDP blocked by firewall | Allow UDP traffic or use same network |
| Stream keeps reconnecting | Unstable UDP path | Check network stability |
| "Connecting..." forever | NAT traversal failed | Use VPN or same network |
| Black video with no error | Firewall dropping UDP | Configure firewall to allow UDP |

---

## Browser Requirements

For best compatibility with video streaming features:

| Browser | Support |
|---------|---------|
| Chrome | Fully supported |
| Firefox | Fully supported |
| Edge | Fully supported |
| Safari | Supported (may require permissions) |

!!! tip
    If video streaming doesn't work, try a different browser or check that your browser allows the required permissions (camera/microphone permissions may be requested for peer connection setup).

---

## Summary

| Feature | Requires |
|---------|----------|
| Web UI Access | TCP port 8888 open |
| Live Video Streaming | UDP traffic allowed |
| Remote Access | Port forwarding, VPN, or same network |
