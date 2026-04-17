<div align="center">

# XIAOZHI-ESPHOME

**Use your Xiaozhi AI devices in ESPHome as voice assistant satellites for Home Assistant.**

<img width="400" src="https://github.com/user-attachments/assets/c26e9291-bd02-4570-8e08-0bee412e07c3" />
<img width="400" src="https://github.com/user-attachments/assets/0c159139-1c2e-45fe-99f2-6b4a258c1dbe" />

<br/><br/>

<img width="300" src="https://github.com/user-attachments/assets/30aff335-13c8-4fc3-9cef-22a0d819d069" />
<img width="300" src="https://github.com/user-attachments/assets/c7092055-329a-4471-8c25-b0718e4b2d54" />

<br/><br/>

<img width="300" src="https://github.com/user-attachments/assets/fe149570-bc99-4d8c-b8fc-365cf61f82e7" />
<img width="300" src="https://github.com/user-attachments/assets/5cf1fd2d-3094-418d-a7cf-882b723aad3d" />

<br/><br/>

<img width="600" src="https://github.com/user-attachments/assets/b66981ab-cdde-4507-821a-26e8d3c57ad6" />

</div>


By request, this GitHub project provides a simple way to use Xiaozhi-based devices with ESPHome. These compact devices can serve as voice assistants integrated with Home Assistant.

## 🚀 Quick Start Guide

### Step-by-step Installation:

1. **Connect your device** to your computer via USB. Open [ESPHome Web](https://web.esphome.io), click “**+ NEW DEVICE**”, and follow the prompts to set it up and connect it to Wi-Fi.
2. In ESPHome Builder, **take over the newly discovered device**, edit the configuration, paste in the code for your device but keep the original device `name`. (You can customize the `friendly_name` as desired.)
3. **Save and install** the configuration **wirelessly**. Wait for it to reboot and begin running your code.
4. Once it’s online, go to **Home Assistant > Devices**, and **accept the new device**. This will start the voice assistant setup process.

> **Note for Step 3:**
> If wireless installation fails and you're prompted to use USB flashing:
>
> * Reconnect the device to your computer if needed.
> * Save and install again, choose “Plug into this computer,” wait for the firmware to compile, download, and use ESPHome Web to install it via USB.
>   This only happens the first time, when the partition table needs to be updated. Future updates can be done wirelessly.

Video going through the esphome install of device was removed by youtube and my account blocked. strange world we live in.

[▶️ Download the install video](https://github.com/RealDeco/xiaozhi-esphome/raw/main/install-xiaozhi-esphome.mp4?raw=true)


## ✅ Supported Devices (so far)

* Espressif EchoEar v1.0
* Espressif EchoEar v1.2
* Spotpear Ball v1
* Spotpear Ball v2
* Spotpear Muma Box v1
* Spotpear Muma Box v2
* Spotpear Muma Horse v1
* Spotpear Muma Horse v2
* Spotpear Puck
* DIY (breadboard)
* Guition 1.8" Taichi pi (JC3636W518C) v1 (discontinued after july 2025)
* Guition 1.8" Taichi pi (JC3636W518C) v2
* Xingzhi Cube 1.54
* "Breadboard Mini", the $7 custom ESP32-S3 with everything onboard
* Waveshare 2.06" OLED Wrist Watch
* Waveshare ESP32-S3-Touch-LCD-1.85C v1
* Waveshare ESP32-S3-Touch-LCD-1.85C v2
* Waveshare ESP32-S3-Touch-LCD-3.49
* Waveshare ESP32-P4-86-Panel-ETH-2RO **NEW**
* Waveshare Audio Board **NEW**
* Guition 4.3C **NEW**
* Guition 7.0 **NEW**
* Freenove 2.8 **NEW**
* Waveshare amoled 1.75 **NEW**
* Xingzhi Cube 1.83 2-Mic **NEW**

(New devices appear in the [modular version](devices/Under_Development/Modular)
 first during testing.)

## 🛒 Where to Buy

EchoEar: https://www.aliexpress.com/item/1005009834934442.html

Ball v1 & v2: https://vi.aliexpress.com/item/1005008627679270.html

alternative link: https://www.aliexpress.com/item/1005009762104155.html

Muma Box: https://vi.aliexpress.com/item/1005009043526078.html

Muma Horse: https://vi.aliexpress.com/item/1005008884232596.html

Puck: https://www.aliexpress.com/item/1005009016529496.html

Guition Taichi pi: https://vi.aliexpress.com/item/1005007420092928.html

Xingzhi Cube 1.54: https://www.aliexpress.com/item/1005008565082769.html

Breadboard: Look in devices/Breadboard: https://github.com/RealDeco/xiaozhi-esphome/tree/main/devices/Breadboard

Breadboard Mini: https://www.aliexpress.com/item/1005009448496585.html

Waveshare 2.06" OLED Wrist Watch: https://vi.aliexpress.com/item/1005009516438849.html

Waveshare ESP32-S3-Touch-LCD-1.85C: https://www.aliexpress.com/item/1005008634826817.html

Waveshare ESP32-S3-Touch-LCD-3.49: https://www.aliexpress.com/item/1005009894437640.html

Waveshare ESP32-P4-86-Panel-ETH-2RO: https://www.aliexpress.com/item/1005009197112179.html

Waveshare Audio Board: https://www.aliexpress.com/item/1005009819735457.html

Guition P4 4.3C: https://www.aliexpress.com/item/1005009673625472.html

Guition P4 7.0: https://www.aliexpress.com/item/1005010022828767.html

Freenove 2.8": https://www.aliexpress.com/item/1005009876628479.html

Xingzhi Cube 1.83 2-mic: https://www.aliexpress.com/item/1005010442374066.html

Waveshare amoled 1.75: https://www.aliexpress.com/item/1005008989323572.html

## Accesories:

Back side case for the 4" (wall mount) nice for desktop use.
https://www.aliexpress.com/item/1005006149480110.html

3D file of "Eggvenger" figure used to hold the Ball in image above, use 115% for v2 since it's larger than v1.
https://makerworld.com/en/models/1238732-eggvenger-superhero-egg-holder

3D file for Wireless charger stand for the Guition JC3636W518 display
https://makerworld.com/en/models/238543-wireless-charger-holder

Wireless charger for the Guition JC3636W518 display: https://vi.aliexpress.com/item/1005005066837741.html

Curled audio cable for Guition JC3636W518 display: https://vi.aliexpress.com/item/1005007061609551.html

Case for Freenove 2.8" (remember to pick correct print profile) https://makerworld.com/en/models/1382304-aura-smart-weather-forecast-display

Other 3D files here: https://github.com/RealDeco/xiaozhi-esphome/tree/main/3D_Files

---EOF

