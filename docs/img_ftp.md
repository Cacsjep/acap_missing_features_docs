# Image to FTP Configuration

Record JPEG frames to the SD card and upload them to the FTP server when a trigger occurs.


!!! danger "SD card"
    *An SD card is required.*

---

#### Access the Configuration Page

- **Open your AXIS camera’s web interface** and navigate to the APP section.
- **Find Missing Feature ACAP:**  
  Open it and enable **Image to FTP** from the feature list to open its configuration panel.

---

#### Configure FTP Server Connection

- **FTP Server:**  
  Enter the FTP server address in format (e.g., 10.0.0.38:21).  
  Include the port like default FTP port 21.

- **FTP Username:**  
  Provide the username for FTP server authentication.

- **FTP Password:**  
  Enter the password for the FTP user account.

- **FTP Directory:**  
  Specify the target directory on the FTP server where images will be uploaded.  

---

#### Configure Storage and Performance Settings

- **Max Size (GB):**  
  Define the maximum storage space (in gigabytes) on the SD card for the image ring buffer.  
  Range: 1-512 GB

- **Max Time (Minutes):**  
  Set the maximum life time in minutes for an image before it's deleted from the local buffer.  
  This helps manage storage space efficiently.

- **Channel:**  
  Select the camera sensor or view area you want to capture images from.  
  Choose the appropriate channel if your camera supports multiple channels.

- **FPS:**  
  Enter the number of frames per second to store locally.  

- **Batch Size (Frames):**  
  Set how many frames are stored in batches to reduce SD card I/O.  
  Using a higher batch size is recommended for better performance, as long as camera RAM allows.  

---

#### Set Up the Trigger Event

- **Configure Trigger Event:**  
  Click the **CONFIGURE TRIGGER EVENT** button to set up when image capture should be triggered.

- **Manual Trigger:**  
  Use the **MANUAL TRIGGER** option for testing or manual activation of the image transfer process.

- **Save Configuration:**  
  Click **SAVE** to store all your settings.

- **Test Connection:**  
  Use the **TEST** button to verify your FTP server connection after saving the configuration.


#### FTP Server Requirements

Before configuring the Image to FTP feature, ensure your FTP server meets the following requirements:

##### Server Compatibility
- **FTP Protocol Support:** Standard FTP (File Transfer Protocol) server
- **Authentication:** Username/password authentication required
- **Anonymous Access:** Supported (use "anonymous" for both username and password)
- **Directory Access:** Server must allow file uploads in the specified target directory

##### Network Configuration
- **Port Access:** Default FTP port 21 must be accessible from the camera
- **Custom Ports:** Supported - specify port in server address (e.g., `192.168.1.100:2121`)
- **Firewall Settings:** Ensure FTP traffic is allowed through your network firewall
- **Passive Mode:** The FTP client operates in passive mode for better compatibility with NAT/firewall configurations

##### Required Permissions
- **Write Access:** FTP user must have write permissions in the target directory

##### File Naming Convention
Uploaded images follow this naming pattern:
```
DD.MM.YYYY_HH_MM_SS.mmm.jpg
```
Example: `09.06.2025_14_30_25.123.jpg`

!!! info "FTP Server Examples"
    Popular FTP servers that work well with this feature include:
    - **FileZilla Server** (Windows/Linux)
    - **vsftpd** (Linux)
    - **Pure-FTPd** (Linux)
    - **ProFTPD** (Linux)
    - Most NAS devices with built-in FTP servers

!!! warning "Security Considerations"
    - FTP transmits credentials in plain text. SFTP can implemented if customer needs it.
