# ESP32-S3-Box-3 LVGL Firmware 
ESPHome Configuration for the S3-Box-3 with an LVGL UI.

All credit and inspiration for the original UI and device configuration goes to https://github.com/BigBobbas/ESP32-S3-Box3-Custom-ESPHome/.

This configuration has also been ported to the P4-86 https://github.com/chrisdunnname/esphome-p4-86-panel-eth-2ro-lvgl.

This firmware provides the S3 box 3 with a voice assistant, timers, screen saver with analog/digital clock and sleep, 12/24 hour time, media controls, radar/presence sensing, temperature (in Celsius or Fahrenheit) and humidity sensing, battery levels and indicator, alarmo integration, alarm clock, internal and external audio, notifications with sound, and multiple pages for lights, thermostats, switches, media, scenes, locks other devices from your Home Assistant.

A large number of entities are exposed to Home Assistant including a notification text entity that provides the ability to push notifications to the device which will display on screen for 10 seconds (with an optional notification sound) and an Image URL entity that provides the ability to push the URL for a PNG image to the device which will display on screen for 30 seconds (with no notification sound). This can be used with a [JPG to PNG Converter](https://github.com/youkorr/hacs-jpg-to-png-converter) in an automation to capture a snapshot from a camera and push it to the device.

A UI Mode feature provides the ability to switch the user experience with 3 modes: Default, HAL, Home.
Each provides a different theme and voice assistant interface.
- Default provides the original theme and a more standard Home Assistant experience. 
- HAL provides a darker theme and an animated Space Odyssey inspired 2001 HAL voice assistant.
- Home provides a more subtle theme and an animated Google inspired voice assistant.

A no animation YAML configuration is provided that provides the UI Mode but without the animated voice assistants for much smaller firmware.

The wakeword is not tied to the UI Mode providing flexibility for your preferred experience.

The On Device Wake Word includes the standard ESPHome wakeword models (4) but also some experimental models including okay hal and hey luna.
Additional experimental wake words for okay computer and hey home assistant and included in the config but disabled by default. 

The UI and Voice Assistant experience is implemented out of the box.
Only basic configuration is required to integrate the standard features with your Home Assistant.

Key Components:
- Radar: https://esphome.netlify.app/components/at581x
- Temperature & Humidity: https://esphome.io/components/sensor/aht10.html (but using AHT20 variant)
- Touchscreen: https://esphome.io/components/touchscreen/gt911.html
- Audio ADC: https://esphome.io/components/audio_adc/es7210.html
- Audio DAC: https://esphome.io/components/audio_dac/es8311.html
- Wake Word VA: https://github.com/esphome/wake-word-voice-assistants/
- Micro Wake Word Models: https://github.com/esphome/micro-wake-word-models/
- LVGL: https://esphome.io/components/lvgl/

# Requirements
Requires [S3-Box-3](https://www.espressif.com/en/dev-board/esp32-s3-box-3-en).

Sensor dock & battery are optional to use radar/presence, battery level and temperature/humidity functionality.

The minimum supported ESPHome version is 2026.1.0.
Last tested on Home Assistant 2026.1 and ESPHome Version 2026.1.

# Loading
![loading](https://github.com/user-attachments/assets/55e0a1b8-8873-42a3-864f-297fa6826b6e)

# Home
**Default UI Mode**

![home_default](https://github.com/user-attachments/assets/54e5eae1-fa0b-4545-9eec-5a903b5f264f)

**HAL UI Mode**

![home_hal](https://github.com/user-attachments/assets/21a0d09a-dbd0-4998-8ca4-be346b89780c)

**Home UI Mode**

![home_home](https://github.com/user-attachments/assets/6fffa773-5d6b-4262-989d-89adf5a64335)

**Home Page Functions**

- The page title is configurable.
- Tapping on the time will show the alarm clock page.
- An alarm clock indicator will appear when the alarm clock is enabled and will launch the alarm settings page. 
- A timer status indicator will appear while a timer is running and will launch the time remaining page
- The alarm status indicator shows current alarm status and launches the security page
- The microphone indicator is a mute switch
- The home assistant indicator indicates if the API is connected
- The wifi status indicator indicates if the WIFI is connected and launches the wifi page
- The battery indicator shows battery charge level or shows a plug indicator if using direct power

- The red circle button on the device will return to the home page, stop a ringing timer, stop the voice assistant and stop any media.
- The top left button on the device will switch from Home page to screensaver, stop a ringing timer, or a long 10s press will perform a factory reset.
- The bottom left button on the device will reboot the device. 

# Voice Assistant
![voice](https://github.com/user-attachments/assets/8c36d316-00ce-40dd-b0ed-fcafdacbfcda)
- Standard experience in Default UI Mode

**Voice Timer Started**

![voice_timer](https://github.com/user-attachments/assets/51b9e7ec-b701-449e-9680-143d9a2e822d)
- Standard experience in Default UI Mode

**Voice Assistant in HAL UI Mode**

https://github.com/user-attachments/assets/6a4cc3aa-5d56-4a20-91e0-ba3e1859db3c

**Timer Time Remaining**

![timer_remaining](https://github.com/user-attachments/assets/347f669a-3fff-4e83-b83f-a98d6d2b5891)

**Timer or Alarm Finished**

![timer_finished](https://github.com/user-attachments/assets/35af66c8-e5af-488e-867e-1dd06876b234)
- Snooze button only shows for Alarm (snooze duration can be set in Home Assistant - default 5 minutes)

# Alarm Clock
![alarmclock](https://github.com/user-attachments/assets/13ae3d0a-0148-443d-95fc-102776ebabb8)
- Access this page by clicking on the toolbar clock. Allows enabling alarm and setting a time.
- Presence snooze will trigger the ringing alarm clock to snooze only once when motion is detected. 
- Alarm Clock can be turned off from same page or is available as a switch so it can be turned off from voice assistant.
- Snooze duration can be set in Home Assistant (default 5 minutes).

# Climate

![climate](https://github.com/user-attachments/assets/688cad8e-7944-4033-bfe1-401e1b1d1427)
- The page title is configurable.

**Air Conditioner**

![climate_ac](https://github.com/user-attachments/assets/e9c9fafe-a0a3-4cb6-83e3-66cfaee51f63)
Tap the change temperature buttons to move slightly or long press to change faster.

# Lights

![lights](https://github.com/user-attachments/assets/d69453d7-d109-435d-addb-da4dd86d2fae)
- The page title is configurable.
- The lights page provides buttons to toggle six configurable lights which can be any entity that supports a toggle function including switch, light, media_player, climate, fan, humidifier, cover, script or siren.

# Controls

![controls](https://github.com/user-attachments/assets/9b612a6d-4c20-4df4-babb-dce395eaa85f)
- The icon of this home page button and the page title are configurable.
- The controls page provides buttons to toggle configurable controls which can be any entity that supports a toggle function including switch, light, media_player, climate, fan, humidifier, cover, script or siren. Icons can be configured for on and off states and must exist in the icon glyphs (default is switch icon).
- The second row of controls buttons include an additional substitution to allow more complex implementations where the sensor for the state is a different entity to the one for the switch. If both the action and state entity are the same for a button these operate the same as the other controls. These three controls are configured with icons to show a garage door, dishwasher and a vacuum cleaner.

# Media
**Internal Audio**

![media_green](https://github.com/user-attachments/assets/0634c06b-d91e-40cc-b3f6-841f1e569ac5)
![media_red](https://github.com/user-attachments/assets/d887ca67-d660-4fc7-b403-6f7a36013427)
- The page title is configurable.
- The volume controls on the media page control the volume of the S3 box including the Voice Assistant.

**External Audio**

![media_exterrnal](https://github.com/user-attachments/assets/cc9af414-de2a-4832-8bc7-78d8c03b7771)

# Screens
![screens](https://github.com/user-attachments/assets/304c2778-a0d5-4183-9f65-31bf96287a61)
- The icon of this home page button and the page title are configurable.
- This page provides buttons to toggle six configurable items which can be any entity that supports turn_on, turn_off functions including switch, light, media_player, climate, fan, humidifier, cover, script or siren. Icons for the On and Off state can be configured for each entity and must exist in the icon glyphs (default is screen on/off icons).

# Security
![s3_security_new](https://github.com/user-attachments/assets/2208367a-a3d0-42c5-906f-5b87edaea400)
- The page title and code page title are configurable.
- The security page provides buttons to toggle three security items. These will work for locks or any other entity that supports a toggle function including switch, light, media_player, climate, fan, humidifier, cover, script or siren. Icons for the On and Off state can be configured for each entity and must exist in the icon glyphs (default is padlock lock/unlock icons).
- Icons on this page are red for off and green for on to indicate security status.
- Keypad - Pin code is required for alarm deactivation or changing modes, but not activation.
- The device background which defaults to black will change color based on alarm status. The colors are configurable.

![keypad](https://github.com/user-attachments/assets/38949e87-908c-4192-a79f-7000efd451c5)

# Screensaver
![saver_analog](https://github.com/user-attachments/assets/eaf5b57a-bd56-4a2e-b661-d73f0000fd17)
![s3_digital_screensaver](https://github.com/user-attachments/assets/d70aab79-27c3-42ab-b382-856521651d64)

- The page title is configurable.
- The top left corner of the screen shows device Temperature and Humidity if a Sensor Dock is connected. Alternatively room temperature and humidity sensors can be defined in substitutions. These additional substitutions will show temperature or temperature and humidity based on the available entities.
- The top right side of the screen shows your specified weather entity temperature and condition and will not be shown if a valid entity is not specified.  Forecast outside temperature and condition can be provided by Open Weather Map or another weather forecast entity in HA.
- The lower left and right corners include can be configured via substitutions to show any sensor value and any unit of measure. For example these could be air quality indicators. These only show if available. 

# Settings
![settings](https://github.com/user-attachments/assets/9f51c38d-4a3f-450a-9771-42b6d3ad4c6e)

**Voice Settings**

![voice_settings](https://github.com/user-attachments/assets/00cc9687-eafa-4b36-a2cc-2d3736bf3ed7)
- WakeWord Location can be changed from On Device to Home Assistant
- Mute Responses will mute any response
- Wake Sound will play a wake sound when voice assistant wakes
- Show Responses will display the conversation on the voice assistant screens.

**Screensaver Settings**

![screensaver_settings](https://github.com/user-attachments/assets/e927612b-085f-46b5-8485-295f39fc37e4)
- Screensaver turns on screensaver after the specified delay
- Settings opens next page to adjust timeouts
- Wake on Presence wakes the device when the radar detects motion
- Turn Screen Off turns off the screen after the specified delay

**Screensaver Timeout Settings**

![screensaver_timeout_settings](https://github.com/user-attachments/assets/c67831bf-be22-426d-b75a-d9858188700b)
- Delay Secs is the time until the screensaver starts
- Screen Off Delay is the time until the screen turns off.
- The brightness slider allows setting the screensaver brightness.

**Info**

![settings_info](https://github.com/user-attachments/assets/39b2682f-fff6-46a9-a8d3-f61043336ae7)

**Device Settings**

![device_settings](https://github.com/user-attachments/assets/b24b582c-0db6-4d58-98a6-bfa76cfa2d22)
- External Audio outputs audio to the external audio player configured in the substitions section.
- Notify sound triggers a sound when a text notification is sent to the device. This is different to the wake sound.
- Stop Ring on Presence will stop any ringing timer or alarm clock when motion is detected. If used with Presence snooze then after snooze complete on next ring motion will stop alarm clock.
- Temperature Offset provides a configurable offset for the temperature sensor that is always set in Celsius.
- Brightness slider controls screen brightness

**UI Settings**

![uisettings](https://github.com/user-attachments/assets/dfa35db8-f2c6-49d7-8f5f-d13f26713a55)
- Time Format provides 12 or 24 HR time shown in all places
- ScreenSaver Clock allows Analog or Digital clock to be shown
- Temperature Unit updates all displayed temperatures to be Celsius or Fahrenheit
- UI Mode provides the ability to choose the theme and corresponding voice assistant experience.

# Wifi
![wifi](https://github.com/user-attachments/assets/b3d620e6-5143-4904-b3ef-78cab5da5b4a)

# OTA Updates
![OTA](https://github.com/user-attachments/assets/7d1bd6bc-216c-4bd0-b760-70dc7f569a53)

# Getting Started

**The basics**

All standard user functions included in the configuration are provided as substitutions in the YAML. 
Under the "Your Configuration Data" section you can change page names, button names, icons and specify entities to enable the standard features of the configuration. 
Only components associated with active entities in your Home Assistant will be shown on the device otherwise these features will be hidden. This means that you can just update the substitutions for the entities you require and only those items will be shown on the associated pages. This makes it easy for anyone to get started with this configuration in a few minutes. For those deploying to multiple S3 devices this new design makes it easier to deploy the same configuration with different entities and makes it easy for those who wish to separate the substitutions from the core yaml.

In ESPHome under secrets ensure you have defined three secrets:
- wifi_ssid (your wifi SSID)
- wifi_password (your wifi Password)
- wifi_qr (a QR code string)

The wifi_qr provides a Wifi QR Code so that guests can scan your screen to get a link, a message or directly connect to your network.
To provide a QR to connect to your wifi the string should be populate like this: "WIFI:S:your_wifi_ssid;T:WPA;P:your_wifi_password;H:false;;". This is the standard format for Wifi QR codes and more details can be found in the WPA3 specification.
Alternatively a text string can be used for a message or a hyperlink can be provided to open a dedicated page when the QR code is scanned.

**Additional Information**

To use the wake, notify and timer sounds with external audio you will need to load the sounds folder to the www folder in your home assistant. This can be done through an add on like Filebrowser or SambaShare.

API encryption is now standard. You can modify the key from your own config or generate one from the Home Assistant Native API web page. 

Binary Sensors or sensors with an on/off status will work as entities for the lights, controls and scenes pages to show status. Users can still press the buttons and they may appear to turn on/off but they won't do anything and any status update will make the indicator show the HA status. 

For triggering more complex scenarios, helper Toggles (input_boolean) and automations can be really useful. You can set an input_boolean as the entity for any of the configurable buttons. You can create an automation using the input_boolean turning on as the trigger, have it perform some actions and then set the input_boolean to off. On the S3 pushing the button will turn this on, triggering the automation and showing the on status and when the automation completes it will go back to off. 

If you are stuck or unsure or have a suggestion raise an issue or reach out as this configuration is the result of requests from fellow users and benefits from your support and involvement. 
