#  Event Camera Bridge Configuration

Register events from other AXIS cameras into the camera rules system to enable cross-camera rules.

---

####  Access the Configuration Page

- **Open your AXIS camera’s web interface:**  
  Enter your camera’s IP address in a web browser.
- **Navigate to the APP Section:**  
  Click on the APP section.
- **Enable Event Camera Bridge:**  
  Locate **Missing Feature ACAP**, open it, and enable **Event Camera Bridge** from the feature list. This opens its configuration panel.

---

!!! warning "Authentication Requirement"
    Only **Digest Authentication** is supported for external cameras.  
    Ensure that your cameras are running an **up-to-date firmware**, and that **digest authentication is enabled**.

#### Configuration

##### Add a Camera  
- **Name:**  
  Enter a descriptive name for the camera.

- **IP Address:**  
  Provide the network IP address of the camera.

- **Username:**  
  Enter the username that the system will use to connect to the camera.

- **Password:**  
  Provide the corresponding password for the specified username.


##### Camera Buttons

**Edit Events**

![](images/conf.PNG)

**Remove Camera**

![](images/rem.PNG)

##### Configure Camera Events

To configure camera events, click the corresponding **Edit Events** button.

![](images/conf.PNG)

###### Add Event

Add a new event for a camera by clicking the **ADD EVENT** button.

![](images/addevent.PNG)

###### Configure Event

Select the desired event. If the event has multiple sources, please specify them. When you are finished, click the **Done** button.

![](images/event.PNG)

##### Save

After adding your cameras and events, click the **SAVE** button.  
If you receive any errors, such as **unauthorized**, verify your username, password, or, in the case of network-related issues, check your camera.


!!! danger "All cameras must be online"
    After saving, a connection test is automatically performed for all external cameras  
    to verify accessibility and credentials.  
    **Make sure all cameras are online during configuration.**

#### Use in AXIS Rules

Once you have configured and saved the settings, you can use the event from other cameras to create a rule.

![](images/rule.PNG)