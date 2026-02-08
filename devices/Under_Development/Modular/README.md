
<img width="1600" height="950" alt="esphome-yaml-layout-pro-v8" src="https://github.com/user-attachments/assets/d9d11428-0f0f-4900-b30c-21c989c6bfba" />

This section is for users who have **already tried the standard full YAML**, followed the installation instructions on the main page, and are familiar with the general setup process.
Because of that, the instructions here are kept to the **bare minimum**.

It’s also the place where **new boards** are introduced first—often in an **alpha or beta stage**—before eventually being added to the main device list (when time allows).

👉 **If you try this and run into problems, please return to the known-working non-modular YAMLs.**
If you believe the issue isn’t just user error, please report it. 😉


---

```markdown
## 🚀 Installation

1. Copy the YAML for your device (e.g. `Muma.yaml`) into your **ESPHome** configuration.  
   - Keep your own `name:` and `friendly_name:` values.  

2. Install the firmware to your device.  

3. (Optional) Update the **settings** section of the YAML if you want to:  
   - Change the `imagemodel`  
   - Change the `startup_sound`  

4. The **clockbackground** can be set directly from **Home Assistant (HA)**.

After this, if there is a update, all you need to do is compile again.
```

```markdown
## 🚀 Running locally (optional)

If you want to run and edit the Modular files locally:
1. Create a directory in your esphome/ directory called "localtest" and copy core.yaml + display_pages.yaml +
clocks-standard.yaml there.
2. Create a directory inside localtest/ called "HW" and copy the hardware file for your device there (eg. ball_v2_hw.yaml)
3. change the packages in main yaml on the device to:

packages:
  core: !include localtest/core.yaml
  hw:   !include localtest/HW/ball_v2_hw.yaml

```
Changes:
---

### Modular v1.12

* **Changes**

  * Integrated the new **SENDSPIN** protocol for a limited number of devices for those who want to test it.
    Currently supported:

    * Guitions 1.80" Taichi Pi (V1)
    * Guitions 1.80" Taichi Pi (V2)
    * Spotpear Ball V2
    * Breadboard Mini aka. "little purple"
    * Waveshare 1.85C (V1)
    * Guition P4 4.3
    * Waveshare P4 4.0 86-ETH
    * Waveshare 1.75 Amoled
    * Waveshare Audio Board
    * Muse Proto


  * These devices were selected due to the popularity of the *Ball* and the usability of the Guitions boards (stereo jack output), and breadboard mini because it's so cheap everyone can play along.

  * If you have a specific device from the project that you’d like to test, feel free to ask.

  * Supports multi-room syncing, album cover art, artist name, and track title display.

### Modular v1.11
- **Changes**
  - added more option to main yaml:

    screensize: this is for compability with larger displays, should be the same as imagesize for smaller displays.

    imagepath: either github or local so it's easier to add own images. this path affect both model images and wallpaper.


### Modular v1.10
- **Changes**
  - added EVENTS for phase id, request text and response text, this makes it easier to play audio on another mediaplayer, or show
    graphics on other displays or dashboards.
  - fixed pipeline to react the same with both single and dual i2s devices, sometimes if would show idle when listening.
  - added the ESP32-P4 series to the family, adjusting clocks to this bigger size displays will be a work in progress.
  - moved clock and wallpaper selectors to external file, so very small or very wide displays can have their own.

### Modular v1.09
- **Changes**
  - minor changes and bugfixes

### Modular v1.08
- **Changes**
  - Updated most devices for mipi spi display drivers

### Modular v1.07
- **Changes**
  - Updated most devices with Music Assistant support

### Modular v1.06
- **Changes**  
  - Fixed swipes didn't work when "handle swipes locally" switch was off, optimized timing
  - changed minor non-breaking spelling mistake

### Modular v1.05
- **Changes**  
  - Added Rolex clockface (wallpaper) for analog clock
  - Fixed Battery status not showing on analog clock and presence (if enabled)

### Modular v1.04
- **Changes**  
  - Added Analog clock (yeah i know, why not)
  - Added switch for doing swipe actions internally (for better first experience), turn off if using automations.
    hardcoded swipe functions: left & right change clock style, down toggle battery, up change wallpaper.
  - Added light sensors for all 5 rooms and changed so presence shows a red LED dot. if light is on in room color is filled.

### Modular v1.03
- **Changes**  
  -  added 2 more clocks, a presence sensor clock and a weather clock

### Modular v1.02
- **Changes**  
  -  added 4 binary sensors for screen swipes, these do nothing by themself but are available in HA for whatevery you desire.
  -  Example automation that changes clock type (left and right swipes), changes wallpaper (swipe up) and toggles battery status (swipe down) can be found in: /Under_Development/Home_Assistant_Automations/
  -  I did it this way so it's not a locked functionality and we can do whatever we want with the sensors.

### Modular core v1.01
- **Fixes**  
  - fixed timer widget, fixed clock screensaver so it doesn't go on when timer is running
  - fixed so it also doesnt show clock until api image timer is done.
- **Changes**  
  - changed font for default clock from 30 to 42, and date font from 18 to 22
  - changed timer widget to new design. and removed 1 font (more memory saving ;)
  - removed removed 1 font, saved memory
  - moved micro wake words to main and left only "okay nabu" in core, this gives a wider selection of mmw's without loading all
---


## Previous Changes (before resetting to Modular 1.00)

### Version 1.06 (dev)
- **Fixes**  
  - Media player buffer set to `250000`  
  - Speaker WAV, mono  
  - Removed `handle_screen_tap` from touchscreen section  

### Version 1.05 (dev)
- Show image API got its own timer  
- Added block so images only display in *idle* or *replying* phase  
- Added switch for time  
- Added clock wallpaper  

### Version 1.04 (dev)
- **Added:** Online clock wallpaper with select in HA  
- **New API service** to send images from HA:

```yaml
service: esphome.esphome_web_0bac48_show_fullscreen_image
data:
  url: "http://homeassistant.local:8123/local/funny.png"
````

### Version 1.03 (dev)

* Minor memory fixes
* Clock select from HA
* Removed one font (req/res font same)
* Removed `alpha_channel`

### Version 1.02 (dev)

* Cleanup and fixes

### Version 1.01 (dev)

* **Added:** Clock when idle for more than 10 seconds
* **Added:** Virtual touch (same functionality as touch available from HA: start, stop, stop timer alarm)
* **Added:** Use original `playing.png` emo when playing music on media player (still reverts to clock after 30s)
* **Added:** Timer ringing entity (can trigger automations)
* **Added:** Longer request/response entities for dashboards (limit 255 characters)
* **Fix:** Timer pages now dynamic for different screen sizes

### Version 1.00

* First working code ✅

