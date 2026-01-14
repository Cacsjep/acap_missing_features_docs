# Notification Service

The Notification Service feature enables sending SMS and voice call notifications through various gateway providers. This is useful for alerting personnel when critical events occur on your Axis device.

---

## Supported Providers

The Notification Service supports three SMS/Voice gateway providers:

| Provider | SMS | Voice Calls | Description |
|----------|-----|-------------|-------------|
| **Seven.io** | Yes | Yes | Cloud-based SMS and voice API service |
| **SMSEagle** | Yes | Yes | Hardware SMS gateway device |
| **Teltonika** | Yes | No | Industrial cellular gateway |

---

## Configuration

### Enabling the Feature

1. Navigate to the **Notification Service** feature in the Missing Features web UI
2. Toggle the feature **Enabled** switch
3. Select your preferred **Provider** from the dropdown
4. Configure the provider-specific settings

### Provider Settings

#### Seven.io

Seven.io is a cloud-based messaging platform that provides SMS and voice call capabilities.

| Setting | Description |
|---------|-------------|
| **API Key** | Your Seven.io API key (obtain from your Seven.io dashboard) |
| **From Number** | The sender phone number or name |
| **To Number** | The recipient phone number (international format, e.g., +43123456789) |

#### SMSEagle

SMSEagle is a hardware SMS gateway that connects to your local network.

| Setting | Description |
|---------|-------------|
| **Host** | IP address or hostname of your SMSEagle device |
| **Username** | SMSEagle API username |
| **Password** | SMSEagle API password |
| **To Number** | The recipient phone number |

#### Teltonika

Teltonika gateways are industrial-grade cellular routers with SMS capabilities.

| Setting | Description |
|---------|-------------|
| **Host** | IP address of your Teltonika gateway |
| **Username** | Gateway login username |
| **Password** | Gateway login password |
| **To Number** | The recipient phone number |

---

## Voice Call Settings

For providers that support voice calls (Seven.io and SMSEagle):

| Setting | Description |
|---------|-------------|
| **Voice Enabled** | Enable/disable voice call notifications |
| **Voice Repeats** | Number of times to repeat the voice message (1-5) |
| **Voice Text** | The text-to-speech message content |

---

## Testing Notifications

Use the **Test SMS** button to send a test message and verify your configuration is working correctly. This will send a test SMS to the configured recipient number.

!!! tip "Test Before Deployment"
    Always test your notification configuration before relying on it for critical alerts. Ensure the recipient receives the test message successfully.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Messages not sending | Verify API credentials and network connectivity to the provider |
| Invalid phone number | Ensure phone numbers are in international format (+country code) |
| Provider timeout | Check network firewall rules and provider service status |
| Voice calls not working | Verify voice is enabled and the provider supports voice in your region |
