# Event Trigger Configuration

Provides a user-friendly interface to configure trigger events for your ACAP application. It allows you to select an event from a dynamically loaded list, customize its source and data fields.

!!! tip 
    This input component is used across several features.

---

#### Launch the Configuration Dialog

- **Open the Dialog:**  
  Click the Configure [Trigger Event] button (displayed with an amber fire icon) located on the interface, the name of the button varies.  

!!! warning 
    If no event is selected, a small message is displayed below the button stating:  
        *"No event selected. Please choose one."*

#### Select an Event

- **Event Autocomplete Field:**  
  In the dialog, you will see an autocomplete field labeled **Event**.  
    - Begin typing to filter the list of available events.
    - The list is organized with headers, making it easier to navigate through different event groups.
    - Select the desired event from the list to load its configuration details.

#### Configure Event Details

- **Source Fields and Data Fields:**  
  Once an event is selected, additional configuration options become available:
  - **Source Fields:**  
    These fields represent the event's source data.  
    - Use checkboxes to ignore fields that you do not need.
    - For fields that are not ignored, enter values or choose from a predefined list.
  - **Data Fields:**  
    Similar to source fields, these capture additional data associated with the event.  
    - You can either provide the necessary values or ignore them based on your needs.
  
- **Validation:**  
  The form automatically validates that all required fields are completed. If any required field is missing, a warning message is shown in the dialog.

#### Save Your Configuration

- **Save and Close:**  
  Once all required fields are correctly filled:
    - Click the **Close** button to exit the dialog.
    - The configuration is saved and will be used.

---

#### Example Workflow

1. **Open the Configuration Dialog:**  
   Click the **Configure Trigger Event** button.  
   - If an event is already selected, notice the chip displaying the event name.  
   - If not, read the prompt encouraging you to select an event.

2. **Select and Configure an Event:**  
   Use the autocomplete field to filter and choose your desired event.  
   Adjust additional settings for source and data fields as necessary.

3. **Validate and Save:**  
   Ensure all required


#### Example for Virtual Port 1 (Active)

[![](images/example.PNG)](images/example.PNG)

#### Example Interval Triggers / Recurring Pulse
If you want to take an action, for example every second, go to **AXIS Camera Events/Schedules** and create a schedule with the type set to `Pulse`. Then, within the event trigger, select `Recurring Pulse` and choose the schedule you created.

[![](images/pulse2.PNG)](images/pulse2.PNG)


[![](images/pulse.PNG)](images/pulse.PNG)

#### Videos
[How to setup an Event Trigger](https://youtu.be/18Kz0O5mTog)

[Setup an AXIS Object Analytics Event Trigger](https://youtu.be/5FFJfvaca3E)

