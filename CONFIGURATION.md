# Detailed Configuration Guide

## The basics

All standard user functions included in the configuration are provided as substitutions in the YAML. Under the "Your Configuration Data" section you can change page names, button names, icons and specify entities to enable the standard features of the configuration. Only components associated with active entities in your Home Assistant will be shown on the device otherwise these features will be hidden. This means that you can just update the substitutions for the entities you require and only those items will be shown on the associated pages. This makes it easy for anyone to get started with this configuration in a few minutes. For those deploying to multiple devices this new design makes it easier to deploy the same configuration with different entities and makes it easy for those who wish to separate the substitutions from the core yaml.

In ESPHome under secrets ensure you have defined three secrets:

wifi_ssid (your wifi SSID)
wifi_password (your wifi Password)
wifi_qr (a QR code string)

The wifi_qr provides a Wifi QR Code so that guests can scan your screen to get details, be navigated to a link or directly connect to your network. To provide a QR to connect to your wifi the string should be populate like this: "WIFI:S:your_wifi_ssid;T:WPA;P:your_wifi_password;H:false;;". This is the standard format for Wifi QR codes and more details can be found in the WPA3 specification. Alternatively a text string can be used for a message or a hyperlink can be provided to open when the QR code is scanned. Deep-link URLs can use the mobile app URL handler to launch specific dashboards, call services or fire events for users with the mobile app installed. Universal links can be used here to trigger an automation the same way as an NFC tag.

## Common behaviours
While many entities are expected to be configured as switches, lights or devices with an On/Off status as specified in the configuration you can use other entity types for some features to achieve different requirements.

Binary Sensors or sensors with an on/off status will work as entities for the lights, controls and scenes pages to show status. Users can still press the buttons and they may appear to turn on/off but they won't do anything and any status update will make the indicator show the HA status. 

For triggering more complex scenarios, helper Toggles (input_boolean) and automations can be really useful. You can set an input_boolean as the entity for any of the configurable buttons. You can create an automation using the input_boolean turning on as the trigger, have it perform some actions in Home Assistant and then set the input_boolean to off. On the device pushing the button will turn this on, triggering the automation and showing the on status and when the automation completes it will go back to off. 

## Substitutions

### Core
This section is where you configure the device name and friendly name used to identify this device. This is how it will be recognized in Home Assistant and the ESPHome Device Builder.

```
  name: esp32-s3box-3
  friendly_name: ESP32-S3-Box-3
```

This is where you configure the media player to use when you have output audio enabled (do not include mediaplayer at the start of the id). This is usually the media player in the same room that you want audio to be case to instead of playing from this device.

```
  external_media_player: your_media_player  ##change this to your external media player enity_id: do not include media_player.
```

Set the URL to your home assistant host. This is required for playing sounds when using external audio.
To use the wake, notify and timer sounds with external audio you will also need to load the sounds folder to the www folder in your home assistant. This can be done through an add on like Filebrowser or SambaShare.

```
  home_assistant_host: http://homeassistant.local:8123 ##change to the full url or IP of your HA server including port
```

API encryption is now standard. 
The `api_key` is a 32-byte base64-encoded string to be used as the encryption key. You can modify the key from your own config or generate one from the Home Assistant Native API web page and replace it. This is not a long live access token. You'll find more details and a generator in the official esphome documentation: https://esphome.io/components/api/#configuration-variables.

```
  api_key: QhTgdqqJ3XY9DbDuN8NS1+zXUr8NaY83pab4dqgy8Yk= ## Device specific API KEY
```

This is the wifi password for the local access point fallback that is enabled if your device cannot connect to your network. 
This is only used for a direct connection to the device in these situations.

```
  wifi_fallback_pwd: s3box123
```

### Home Page
This is where you configure the home page.
Each button can be individually hidden or have the icon and color changed.
Hiding the page from navigation removes it from the navigation arrows and swiping.

```
  home_page_title: "Home"
  home_page_hide_from_navigation: false
  home1_icon: "\U000F050F"
  home1_color: lightblue
  home1_page: climate_page
  home1_hidden: false
  home2_icon: "\U000F06E8"
  home2_color: yellow
  home2_page: lights_page
  home2_hidden: false
  home3_icon: "\U000F097E"
  home3_color: fuchsia
  home3_page: controls_page
  home3_hidden: false
  home4_icon: "\U000F075A"
  home4_color: red
  home4_page: media_page
  home4_hidden: false
  home5_icon: "\U000F0FCE"
  home5_color: aqua
  home5_page: scenes_page
  home5_hidden: false
  home6_icon: "\U000F033E"
  home6_color: lime
  home6_page: security_page
  home6_hidden: false
```

Other pages that could be used out of the box on the home page are below:
| Page | Use Case |
| -------- | -------- |
| climate_studio_page | Shows climate 1 Air Conditioner |
| saver_page | Show Screensaver |
| settings_page | Show Settings |
| info_page | Show device information |
| wifi_page | Show wifi information |

### Climate Page
This is where you configure the name and the devices that will be displayed on the climate page. The climate1 entity is expected to be a climate device like an airconditioner and includes a detail page to allow setting the target temperature and HVAC Mode. All other entities are expected to be temperature sensors.
Hiding the page from navigation removes it from the navigation arrows and swiping.

```
  climate_page_title: "Climate"
  climate_page_hide_from_navigation: false
  climate1_name: "Room 1 AC"
  climate1_entity: climate.air_conditioner #climate
  climate2_name: "Room 2"
  climate2_entity: sensor.temperature2 #sensor
  climate3_name: "Room 3"
  climate3_entity: sensor.temperature3 #sensor
  climate4_name: "Room 4"
  climate4_entity: sensor.temperature4 #sensor
  climate5_name: "Room 5"
  climate5_entity: sensor.temperature5 #sensor
  climate6_name: "Room 6"
  climate6_entity: sensor.temperature6 #sensor
```

### Lights Page
This is where you configure the name and the devices that will be displayed on the lights page.
These do not have to be lights and can be any device that supports a toggle switch to turn it on or off.
Hiding the page from navigation removes it from the navigation arrows and swiping.

```
  lights_page_title: "Lights"
  lights_page_hide_from_navigation: false
  lights1_name: "Light 1"
  lights1_entity: switch.light1 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  lights2_name: "Light 2"
  lights2_entity: switch.light2 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  lights3_name: "Light 3"
  lights3_entity: light.light3 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  lights4_name: "Light 4"
  lights4_entity: switch.light4 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  lights5_name: "Light 5"
  lights5_entity: switch.light5 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  lights6_name: "Light 6"
  lights6_entity: light.light6 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
```

### Controls Page
This is where you configure the name and the devices that will be displayed on the controls page.
To support all types of devices this page allows you to configure the icon to be shown when in on or off state in addition to the name and entity for each for each button. This will support any device that supports a toggle switch to turn it on or off.
Hiding the page from navigation removes it from the navigation arrows and swiping.

```
  controls_page_title: "Controls"
  controls_page_hide_from_navigation: false
  controls1_name: "Kettle"
  controls1_entity: switch.kettle_start #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  controls1_icon_off: "\U000F131B" #icon from icon_glyphs
  controls1_icon_on: "\U000F05FA" #icon from icon_glyphs
  controls2_name: "Control 2"
  controls2_entity: switch.switch2 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  controls2_icon_off: "\U000F097E" #icon from icon_glyphs
  controls2_icon_on: "\U000F097E" #icon from icon_glyphs
  controls3_name: "Control 3"
  controls3_entity: switch.switch3 #switch,light,media_player,climate,fan,humidifier,cover,script,siren
  controls3_icon_off: "\U000F097E" #icon from icon_glyphs
  controls3_icon_on: "\U000F097E" #icon from icon_glyphs
  controls4_name: "Garage"
  controls4_entity: switch.garagedoorbutton #toggle - switch,light,media_player,climate,fan,humidifier,cover,script,siren
  controls4_state: binary_sensor.garagedoorstate #status - switch,light,fan,cover,sensor,binary_sensor
  controls4_icon_off: "\U000F12D3" #icon from icon_glyphs
  controls4_icon_on: "\U000F12D4" #icon from icon_glyphs
  controls5_name: "Dishwasher"
  controls5_entity: input_boolean.dishwasher #toggle - switch,light,media_player,climate,fan,humidifier,cover,script,siren
  controls5_state: input_boolean.dishwasher #status - switch,light,fan,cover,sensor,binary_sensor
  controls5_icon_off: "\U000F0AAC" #icon from icon_glyphs
  controls5_icon_on: "\U000F11B8" #icon from icon_glyphs
  controls6_name: "Vacuum"
  controls6_entity: input_boolean.vacuum #toggle - switch,light,media_player,climate,fan,humidifier,cover,script,siren
  controls6_state: input_boolean.vacuum #status - switch,light,fan,cover,sensor,binary_sensor
  controls6_icon_off: "\U000F1C01" #icon from icon_glyphs
  controls6_icon_on: "\U000F070D" #icon from icon_glyphs
```

### Media Page
The media page does not require configuration and a different page will show depending on whether you are using on device or external audio. Here you can translate or change the page title.  Hiding the page from navigation removes it from the navigation arrows and swiping.  The media switcher is for users who want to be able to control both internal and external audio from the media page. When visible a button is added to the media_page to allow switching between the internal audio player and the external audio player irrespective of whether external audio output is enabled. 

```
  media_page_title: "Media"
  media_page_hide_from_navigation: false
  media_page_hide_switcher: true
```

### Scenes Page
The scenes page is intended to support media devices and can be any device that supports a toggle switch to turn it on or off. These devices require the ability to be turned on or off allowing most media devices to be supported.
Hiding the page from navigation removes it from the navigation arrows and swiping.

```
  scenes_page_title: "Screens"
  scenes_page_hide_from_navigation: false
  scenes1_name: "Player 1"
  scenes1_entity: media_player.media1 #switch,light,media_player,climate,fan,humidifier,cover,script,siren,scene
  scenes1_icon_off: "\U000F083B" #icon from icon_glyphs
  scenes1_icon_on: "\U000F0502" #icon from icon_glyphs
  scenes2_name: "Player 2"
  scenes2_entity: media_player.media2 #switch,light,media_player,climate,fan,humidifier,cover,script,siren,scene
  scenes2_icon_off: "\U000F083B" #icon from icon_glyphs
  scenes2_icon_on: "\U000F0502" #icon from icon_glyphs
  scenes3_name: "Player 3"
  scenes3_entity: media_player.media3 #switch,light,media_player,climate,fan,humidifier,cover,script,siren,scene
  scenes3_icon_off: "\U000F083B" #icon from icon_glyphs
  scenes3_icon_on: "\U000F0502" #icon from icon_glyphs
  scenes4_name: "Player 4"
  scenes4_entity: media_player.media4 #switch,light,media_player,climate,fan,humidifier,cover,script,siren,scene
  scenes4_icon_off: "\U000F083B" #icon from icon_glyphs
  scenes4_icon_on: "\U000F0502" #icon from icon_glyphs
  scenes5_name: "Player 5"
  scenes5_entity: media_player.media5 #switch,light,media_player,climate,fan,humidifier,cover,script,siren,scene
  scenes5_icon_off: "\U000F083B" #icon from icon_glyphs
  scenes5_icon_on: "\U000F0502" #icon from icon_glyphs
  scenes6_name: "Player 6"
  scenes6_entity: media_player.media6 #switch,light,media_player,climate,fan,humidifier,cover,script,siren,scene
  scenes6_icon_off: "\U000F083B" #icon from icon_glyphs
  scenes6_icon_on: "\U000F0502" #icon from icon_glyphs
```

### Alarm
Alarm integration was designed for Alarmo but can support other integrations like Envisalink where alarm modes and statuses are consistent. The background settings change the color on all pages to indicate alarm status. When the alarm is off the default background color will be used, but when the alarm is pending, on or triggered the color will change as specified here. This means you can easily see if the alarm is armed.

```
  alarm_entity: alarm_control_panel.alarmo #Alarm - tested with Alarmo
  alarm_background_on: 0x001a00 #Set to 0x000000 for black
  alarm_background_pending: 0x330d00 #Set to 0x000000 for black
  alarm_background_triggered: 0x4d0000 #Set to 0x000000 for black
```

### Security Page
The security page will show the alarm controls but includes three additional buttons that can be any device that supports a toggle switch to turn it on or off. The buttons on this page also support lock/unlock actions for lock devices. 
Activating the alarm does not require a code but a code is required to deactivate. When required a number keypad will display for the user to enter the code. The title of this page can be configured.
Hiding the page from navigation removes it from the navigation arrows and swiping.

```
  security_page_title: "Security"
  security_page_hide_from_navigation: false
  security1_name: "Lock 1"
  security1_entity: lock.lock1 #lock,switch,light,media_player,climate,fan,humidifier,cover,script,siren
  security1_icon_off: "\U000F033F" #icon from icon_glyphs
  security1_icon_on: "\U000F033E" #icon from icon_glyphs
  security2_name: "Lock 2"
  security2_entity: lock.lock2 #lock,switch,light,media_player,climate,fan,humidifier,cover,script,siren
  security2_icon_off: "\U000F033F" #icon from icon_glyphs
  security2_icon_on: "\U000F033E" #icon from icon_glyphs
  security3_name: "Lock 3"
  security3_entity: lock.lock3 #lock,switch,light,media_player,climate,fan,humidifier,cover,script,siren
  security3_icon_off: "\U000F033F" #icon from icon_glyphs
  security3_icon_on: "\U000F033E" #icon from icon_glyphs
  keypad_page_title: "Code"
```

### Screensaver Page
When the screensaver is enabled the screensaver page can show the weather (top right), temperature (top left) and humidity (top left). If using the sensor dock the temperature and humidity from the device will be shown but other users can specify the custom temperature and humidify sensors. This allows showing both in and out temperatures on the screensaver page. 
Additional lower left and lower right labels can be configured to show data from any other sensor. All entities support decimal places substitutions to allow configuration of the granularity of the number to be shown. 

```
  screensaver_page_title: " "
  screensaver_weather_entity: weather.home #weather
  screensaver_weather_dp: 1 #decimal places for weather temperature
  screensaver_temperature_entity: sensor.temperature #temperature sensor for local room
  screensaver_temperature_dp: 1 #decimal places for temperature entity
  screensaver_humidity_entity: sensor.humidity #humidity sensor for local room
  screensaver_humidity_dp: 0 #decimal places for humidity entity
  screensaver_lower_left_entity: sensor.airquality #sensor
  screensaver_lower_left_dp: 0 #decimal places for lower left entity
  screensaver_lower_left_units: " μg"
  screensaver_lower_right_entity: sensor.allergen_index #sensor
  screensaver_lower_right_dp: 0 #decimal places for lower right entity
  screensaver_lower_right_units: " IAI"
```

### Alarm Clock Page
The alarm clock  page does not require configuration and can be accessed by tapping the taskbar clock or the icon shown when an alarm is active. The alarm clock supports two alarms. This option allows you to hide the button for switching between alarm 1 and alarm 2 on the device. This allows you to simplify the device to only have one alarm clock option which will hide the second alarm so users cannot modify it. You will still be able to set both alarms from Home Assistant even if only one is visible. 

```
   alarmclock_hide_switcher: false"
```

### Notification Page
The notification  page does not require configuration and will appear when the device receives notification text. This option allows you to translate or change the page title. 

```
  notification_page_title: "Notification"
```

### Wifi QR Code
This substitution allows you to override the QR codes on the wifi page to use a value instead of the wifi_qr secret. This can be useful if you want to put text or a hyperlink in the QR code instead of providing wifi access. If you are wanting to provide a QR code that allows connecting to wifi you should not change this and should configure this using a secret so your password is not in this configuration.

```
  wifi_qr_code: !secret wifi_qr #QR code for wifi
```

### Default Page
This controls the default page shown when the device starts or wakes from screensaver or when the user presses the home button. This can be changed to default to a different page.

```
  default_page: idle_page #set default idle page e.g. idle_page, lights_page, scenes_page, controls_page
```

### Days and Months
The screensaver shows the date using these parameters. These can be modified for translating the time shown to other languages.

```
###### Days and Months ######
## Change the values on the right to match your locale ##
  sun: Sun
  mon: Mon
  tue: Tue
  wed: Wed
  thur: Thur
  fri: Fri
  sat: Sat
  sunday: Sunday
  monday: Monday
  tuesday: Tuesday
  wednesday: Wednesday
  thursday: Thursday
  friday: Friday
  saturday: Saturday
  
  jan: Jan
  feb: Feb
  mar: Mar
  apr: Apr
  jun: Jun
  jul: Jul
  aug: Aug
  sept: Sept
  oct: Oct
  nov: Nov
  dec: Dec
  january: January
  february: February
  march: March
  april: April
  may: May
  june: June
  july: July
  august: August
  september: September
  october: October
  november: November
  december: December
```
### Images
These substitutions specify the images to be used for core images and default voice animation, the HAL animation (anim_) and the Home animation (anim2_).

```
###### Default Images ######
  loading_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/loading_lvgl.png
  # idle_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/idle.png
  listening_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/listening.png
  thinking_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/thinking.png
  replying_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/replying.png
  error_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/error.png
  # timer_finished_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/timer_finished_320_240.png
  # logo_illustration_file: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/logo.png
  
######  Animated Voice Assistant images ######
  anim_listening1: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/listening_1.png #listening
  anim_listening2: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/listening_2.png #listening, thinking
  anim_thinking1: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/thinking_1.png #thinking
  anim_thinking2: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/thinking_2.png #thinking, replying
  anim_replying1: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/replying_1.png #replying
  anim_replying2: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/replying_2.png #replying
  anim_error1: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/HAL/error_1.png #error
  anim2_listening: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_grey.png
  anim2_thinking: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_white.png
  anim2_replying1: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_colors_1.png
  anim2_replying2: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_colors_2.png
  anim2_replying3: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_colors_3.png
  anim2_replying4: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_colors_4.png
  anim2_error: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/images/Home/g_orange.png
```

### Audio Files
These substitutions specify the sounds to be used for on device or external audio. The on device sounds will be included in the firmware on the device. The external audio sounds need to be available on your Home Assistant server at the specified URLs.

```
######  Audio Files ######
# FLAC files for on device sounds
  timerfinished_ondevice: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/sounds/timer_finished.flac
  wakesound_ondevice: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/sounds/awake.flac
  notify_ondevice: https://github.com/chrisdunnname/esphome-s3-box-3-lvgl/raw/main/sounds/notify.flac
# Home Assistant path to MP3 files for external audio sounds
  timerfinished_external: /local/sounds/timer_finished.mp3
  wakesound_external: /local/sounds/awake.mp3
  notify_external: /local/sounds/notify.mp3
```

### Other Configuration
Other font information and hardware specific configuration is included in substitutions. You should not need to change these unless you are experiencing an issue or are looking to try this configuration on different hardware. 