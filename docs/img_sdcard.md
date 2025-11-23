# Image to SD Card Configuration

*Of course you need an SD-Card.*

---

#### Access the Configuration Page

- **Open your AXIS camera’s web interface** and navigate to the APP section.
- **Find Missing Feature ACAP:**  
  Open it and enable **Image to SD Card** from the feature list to open its configuration panel.

---

#### Configure Storage Limits

- **Max Size (GB):**  
  Define the maximum storage space (in gigabytes) on the SD card for saving images.  
  If the allocated space is reached, the oldest images will be deleted automatically.

- **Max Time (Days):**  
  Specify the number of days to keep images.  
  If images exceed this age, they will be removed to free up space.

- **Channel:**  
  If your camera supports multiple channels or sensors, select the channel for which you want to capture images.

---

#### Set Up the Trigger Event

- **Trigger Event:**  
  Select the event source that will trigger image capture (e.g., Device I/O Virtual Input, Motion Detection, or Analytics).

- **Source/Data Fields:**  
  Depending on your event source, you may need to specify additional parameters (e.g., port number for a virtual input).

- **Save Configuration:**  
  Click **Save** to store your settings.  

#### Storage Info

After saving your configuration, you can view real-time storage details:

- **Max Size / Max Days:** Displays the limits you’ve configured.
- **GB Used:** Shows how much of the allocated storage is currently in use.
- **Oldest Image / Newest Image:** Indicates the timestamps of the oldest and newest stored snapshots.
- **Storage Filled (%):** A quick reference to how much space is occupied.
- **Reload Status:** Click **RELOAD** to refresh the storage status and see if older images have been removed or if new images have been added.

---

#### Images

- **View Images:**  
    Specify a date and time range in the **From** and **To** fields.
- **Search:**  
    Click the search button (magnifying glass icon) to load thumbnails of images captured in that timeframe.
- **Grid Columns:**  
    Adjust the grid layout to see more or fewer images per page.
- **Pagination:**  
    Use the page numbers to navigate through all snapshots.
- **Zoom and Download an Image:**  
    Hover over an image with your mouse to reveal the zoom and download buttons for that image.

##### Generating Videos

!!! info 

    The video generator uses images that have been “searched or viewed” in the **View Images** section. Ensure the time range includes the snapshots you want in your video.

- **Video Generation Settings:**  
    - **FPS:** Frames per second for the resulting video (1–60).
    - **Width / Height:** Dimensions of the output video (e.g., 1280×720).
    - **HTTPS Requirement:** Video encoding requires a secure connection (HTTPS). If you see a warning, reload the camera interface over HTTPS.
- **Convert to MP4:**  
    Click **Convert to MP4** to generate your timelapse or video file from the selected snapshots. The video will be created and made available for download.

!!! warning "HTTPS Required"

    Video encoding requires a secure connection (HTTPS). If you see a warning, reload the camera interface over HTTPS.
