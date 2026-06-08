# Home Assistant Usage Guide

## The basics

This guide details the available Home Assistant entities implemented with this device and how they can be used. 

## Entities

### Controls

| Entity | Use Case |
| -------- | -------- |
| Alarm Clock | Disable or Enable the Alarm Clock | 
| Alarm Clock Time | The set time for the Alarm Clock |
| Media Player | The device media player |
| Volume | The device volumme |
| Notification Text | Text entered here triggers a notification to display on the notification page for 10 seconds |

### Sensors

| Entity | Use Case |
| -------- | -------- |
| Presence Detect | Indicates presence if the sensor dock is connected and radar enabled | 
| S3 Humidity | The device humidity if the sensor dock is connected |
| S3 Temperature | The device temperature if the sensor dock is connected |
| Timer | The countdown of the currently active voice assistant timer |

### Assist

| Entity | Use Case |
| -------- | -------- |
| Assist Satellite | The status of the voice assistant | 

### Configuration

| Entity | Use Case |
| -------- | -------- |
| Assistant | The default assist pipeline to be used by the primary wake word | 
| Assistant 2 | The secondary assist pipeline to be used by the secondary wake word | 
| Default Brightness | The default screen brightness | 
| Display Conversation | Whether conversation text should be displayed on screen when using the voice assistant | 
| Finished Speaking Detection | Controls how the voice assistant decides when you have finished speaking | 
| Firmware | Indicates if pending updates to the current ESPHome version | 
| Image URL | This entity allows you to push a fully qualified URL for a PNG image to the device which will display on screen for 30 seconds (with no notification sound). This can be used with a [JPG to PNG Converter](https://github.com/youkorr/hacs-jpg-to-png-converter) in an automation to capture a snapshot from a camera and push it to the device.|
| LCD Backlight | Allows controlling the backlight for the device | 
| Mute | Mutes the microphone on the device |
| Mute Reponses | Stops the voice assistant from playing TTS responses | 
| Notification Sound | Enables a notification sound when a text notification is sent to the device | 
| Output Audio Externally | Sends all audio to the specified external media player | 
| Play Wake Sound | Enables a wake sound when activating the voice assistant | 
| Presence Duration | How long the device should show occupancy when presence is detected | 
| Radar Enable | Enables the radar if the sensor dock is connected | 
| Reboot Schedule | Enables the automated reboot schedule | 
| Reboot Time | Specifies the time when the device should reboot automatically | 
| Screen wake on motion | Triggers the device to wake on presence if the radar is enabled  | 
| Screensaver Clock | Display an Analog or Digital clock on the screensaver page | 
| ScrOff Delay | Sets the time delay in seconds to turn off the display after activity | 
| ScrOff Enable | Enables the device to turn off the display when no activity | 
| ScrSave Brightness | Sets the brightness of the display when the screensaver is shown | 
| ScrSave Delay | Sets the time delay in seconds to show the screensaver after activity | 
| ScrSave Enable | Enables the device to show the screen saver on the display when no activity | 
| Snooze on Presence | Enables the alarm clock to snooze automatically when presence is detected by the radar | 
| Snooze Time | Sets the time delay in seconds for snoozing the alarm clock | 
| Stop Ring on Presence | Enables alarms and timers to stop ringing when presence is detected by the radar | 
| Temp Offset | Allows calibration adjustment of the sensor dock temperature | 
| Temperature Unit | Controls whether temperature should be shown in Fahrenheit or Celsius | 
| Time Format | Controls whether the clock should show 12 or 24 hour time |
| UI Mode | Selects the appropriate UI theme to be applied |
| VA Mode | Selects the voice assistant animation/images to be shown |
| Wake Word | Sets the on device wake word for the primary voice assistant pipeline |
| Wake Word 2 | Sets the on device wake word for the secondary voice assistant pipeline |
| Wake Word Engine Location | Specifies whether wake word detection is on device or on home assistant |
| Wake Word Sensitivity | Adjusts the sensitivity of wake word detection for the selected wake word models |

### Diagnostic

| Entity | Use Case |
| -------- | -------- |
| Battery Level | The battery level of the battery if the sensor dock is connected | 
| BSSID | The current wifi base station ID | 
| Device Uptime | The current device uptime. This updates every 5 minutes to minimize log data | 
| IP Address | The current IP address of the device | 
| Radar Reset | Resets the radar if required to address presence detection issues | 
| Reboot | Reboots the device | 
| SSID | The current connected wifi network | 
| WiFi db | The signal strength of the current connected wifi access point in decibel-milliwatts | 
| WiFi Signal | The signal strength of the current connected wifi access point as a percentage  | 

## Automations

The above entities unlock additional capabilities within Home Assistant. The following are example use cases:

### Quieter and Easier Notifications

When on a call or with guests TTS announcements can be disruptive. Using the notification text (optionally with a brief notification sound) I can see when someone rings the doorbell or leaves the garage door open using automations to pass a message to the notification text field on all my devices.
An input text helper on my Home Assistant dashboard is also used to trigger an automation that copies the same message as a notification to all my devices - instant family text broadcasts.

### Controlled Reboots

Rebooting devices ensures there are no concerns by freeing up memory and resolving any temporary issues. If you need multiple reboot schedules and want to tailor these to appropriate times of low activity use an automation to press the reboot button.

### Camera Images

When a camera detects motion and captures an image or video you can now display that on your device. An automation can detect motion or a recording and then take a snapshot of the appropriate camera. That snapshot can then be converted to PNG using [JPG to PNG Converter](https://github.com/youkorr/hacs-jpg-to-png-converter) and stored in a folder on HA which is the passed as a URL to the Image URL field of all devices. This means shortly after motion is detected a snapshot of that camera can appear on my device.

### TTS Announcements

As this is a media player and voice assistant you can use any TTS provider to convert calendar entries or notifications to devices as a voice announcement.

### Time of Day Behaviours

The built in screensaer and screen off options provide the ability for the device to wake and sleep as required. These options can also be controlled remotely using an automation to allow devices to turn off the screen when not in use during the day but at night we can turn off the screen off feature to leave the screen on and go to screensaver instead at a dimmer brightness so users have a visible clock. 

### Multiple Assist Pipelines

Using two on device wake words and multiple assist pipelines provides an ability for your device to respond to one command for local control and a second command for using an online agent like Gemini or ChatGPT. 

### Automating UI Modes

An automation can change the UI mode to provide Light and Dark Modes to align with sunset and sunrise. 

## Automated Recovery

The device will automatically reboot if the voice assistant does not successfully complete a response in the expected time or if the touchscreen fails to initialize. These are expected behaviours to mitigate issues that can cause the device to become unresponsive. 