# Event Sunrise and Sunset Configuration

Using SunriseSunset.io API for retrieving sunrise and sunset times for a specific longitude and latitude to create AXIS event triggers.

---

####  Access the Configuration Page

- **Open your AXIS camera’s web interface:**  
  Enter your camera’s IP address in a web browser.
- **Navigate to the APP Section:**  
  Click on the APP section.
- **Enable Event Sunrise and Sunset:**  
  Locate **Missing Feature ACAP**, open it, and enable **Event Sunrise and Sunset** from the feature list. This opens its configuration panel.

---

#### Select Location on Map

- **Select Location:**  
  Click on the map within the configuration panel to choose your camera’s location. This allows the system to retrieve the correct sunrise and sunset times for your area.

---

#### Save the Configuration

- **Save:**  
  After setting the location, click **Save** to apply the settings.

!!! tip 
    After saving your changes, details about the upcoming sunrise and sunset will be displayed beneath the map.

    [![](images/times.PNG)](images/times.PNG)


#### Use with AXIS Rule System

To integrate day/night events from Missing Feature ACAP into your AXIS camera’s rule system, follow these steps:

1. **Add a New Rule**  
      - In the AXIS camera’s web interface, navigate to **System** > **Events** > **Rules** and click **Add a Rule**.

2. **Specify the Rule Name**  
      - Give your rule a descriptive name, for example, **DN Change**.

3. **Set the Condition**  
    - From the dropdown, select **Missing Features: Sun Events**.
    - Choose **Daylight**.

4. **Configure the Action**  
    - In the **Action** section, pick the desired behavior (e.g., **Use day-night mode while the rule is active**).
    - Adjust any additional settings to suit your requirements.

5. **Save the Rule**  
    - Click **Save** to finalize the configuration.

[![](images/rule.PNG)](images/rule.PNG)