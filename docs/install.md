# Installation

The Axis Camera Application Platform (ACAP) enables you to extend your Axis camera’s capabilities with custom applications. This guide provides step-by-step instructions for installing an ACAP application on your camera using several methods.

---

## Prerequisites

- **Compatible Axis Camera:** Verify that your camera supports ACAP.
- **ACAP Application File:** Obtain the `.eap` file for the ACAP application you want to install.
- **Allow unsigned apps:** Enable "allow unsigned apps"
- **Network Connection:** Ensure your computer is on the same network as the camera.
- **Administrator Credentials:** You need admin access to the camera’s web interface.
- **AXIS Device Manager (Optional):** For bulk deployments across multiple cameras. 

---

## Accessing the Missing Features Web UI

Missing Features runs on a **dedicated web server** inside the camera that listens on port `8888` (HTTP + HTTPS are proxied through `https://<camera-ip>:8888`). This server is separate from Axis’s default web interface.

- **Default credentials:** `admin` / `admin`. The login screen enforces a password change the first time you sign in.
- **Secure access:** If you switch to the **Uploaded certificate** mode in Settings, the web server swaps to the custom PEM/key pair only after validation so the UI never restarts with an invalid certificate. If you decide to revert to the built-in self-signed certificate, the mode switch also closes the current TLS connection and reloads with the generated cert.

Make sure `Allow unsigned apps` remains enabled while you upload the `.eap`, and remember to close the dedicated UI when you finish so the camera can continue serving its primary interface.

!!! tip "Enable: Allow unsigned apps"
    [![](images/allow.PNG)](images/allow.PNG)

## Installing via the Camera’s Web Interface

This is the simplest method if you’re installing on a single camera.

1. **Access the Camera:**
      - Open your web browser and navigate to your camera’s IP address (e.g., `http://192.168.1.90`).
      - Log in with your administrator credentials.

2. **Navigate to the Apps Section:**
      - Click on the **Apps** (or similar) section in the camera’s web UI.

3. **Allow unsigned apps:**
      - For now you need to enable "**Allow unsigned apps:**"

4. **Upload and Install the ACAP:**
      - Click **Install Application** (or “Upload ACAP”).
      - Browse to select Missing Features ACAP `.eap` file.
      - Confirm the upload.
      - Once uploaded, the camera will display the application. Click **Start** (or **Activate**) to launch it.

6. **License:**
      - Apply the license.


---

## Deploying with AXIS Device Manager

For installations on multiple cameras, use AXIS Device Manager to streamline deployment.

1. **Launch AXIS Device Manager:**
      - Install and open the AXIS Device Manager on a Windows computer.

2. **Select Your Devices:**
      - Scan your network and select the cameras on which you want to install the ACAP.

3. **Deploy the Application:**
      - Right-click on the selected device(s) and choose **Install Camera Application...**.
      - In the installation wizard, browse for your `.eap` file.
      - If required, select to install any necessary licenses.
      - Click **Finish** to start the deployment.  
      - Monitor the progress in the task list to confirm successful installation.

4. **License:**
      - Apply the license.
