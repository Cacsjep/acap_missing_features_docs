# Installation

The Axis Camera Application Platform (ACAP) enables you to extend your Axis device's capabilities with custom applications. This guide provides step-by-step instructions for installing Missing Features ACAP on your device.

---

## Prerequisites

- **Compatible Axis Device:** Verify that your device (camera or I/O module) supports ACAP
- **ACAP Application File:** Obtain the `.eap` file for the ACAP application
- **Allow unsigned apps:** Enable "allow unsigned apps" in device settings
- **Network Connection:** Ensure your computer is on the same network as the device
- <span style="background-color: #FFA726; color: black; padding: 2px 6px; border-radius: 3px; font-weight: bold;">Port 8888 Access:</span> Missing Features runs its own web server on port `8888`. Ensure this port is accessible from your network (check firewall rules if needed)
- **Administrator Credentials:** You need admin access to the device's web interface

---

## Installing via the Device's Web Interface

This is the simplest method if you're installing on a single device.

1. **Access the Device:**
      - Open your web browser and navigate to your device's IP address (e.g., `http://192.168.1.90`)
      - Log in with your administrator credentials

2. **Navigate to the Apps Section:**
      - Click on the **Apps** (or similar) section in the device's web UI

3. **Allow unsigned apps:**
      - Enable "**Allow unsigned apps**" in the settings

4. **Upload and Install the ACAP:**
      - Click **Install Application** (or "Upload ACAP")
      - Browse to select the Missing Features ACAP `.eap` file
      - Confirm the upload
      - Once uploaded, the device will display the application. Click **Start** (or **Activate**) to launch it

5. **Apply License:**
      - See the Licensing section below

!!! tip "Enable: Allow unsigned apps"
    [![](images/allow.PNG)](images/allow.PNG)

!!! warning "Port 8888 Access Required"
    Missing Features runs its own web server on port **8888**. After installation, access the UI at:

    `https://<device-ip>:8888`

    Ensure this port is not blocked by firewalls. The UI is separate from the standard Axis web interface.

---

## Accessing the Missing Features Web UI

Missing Features runs on a **dedicated web server** inside the device that listens on port `8888` (HTTP + HTTPS are proxied through `https://<device-ip>:8888`). This server is separate from Axis's default web interface.

- **Default credentials:** `admin` / `admin`. The login screen enforces a password change the first time you sign in.
- **Secure access:** If you switch to the **Uploaded certificate** mode in Settings, the web server swaps to the custom PEM/key pair only after validation so the UI never restarts with an invalid certificate.

---

## Licensing

Missing Features ACAP requires a valid license to enable its features—without a license, a feature cannot be configured or started.

### Using a Purchased License

If you have purchased a license:

1. Register it through the [AXIS License Registration](https://www.axis.com/support/license-key-registration) portal
2. Open the device web UI, navigate to the **Apps** section
3. Activate the Missing Features ACAP license

### Requesting a Trial License

For testing and evaluation purposes, demo licenses can be requested by sending an email to [VideoSolutions@commend.at](mailto:VideoSolutions@commend.at) if the trial licenses have expired or their usage limits have been exceeded.
