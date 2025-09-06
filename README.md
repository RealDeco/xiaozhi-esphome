
# XIAOZHI-ESPHOME

**Use your Xiaozhi AI devices in ESPHome as voice assistants for Home Assistant.**

(fully working with on-device wake word and custom graphics.)

<p align="center">
  <img src="https://github.com/user-attachments/assets/958a2d99-5752-481b-be1e-d9ad797e6e33" alt="fam" width="250"/>
  <img src="https://github.com/user-attachments/assets/96f173cd-9d57-492d-906c-83f3ce603ff2" alt="DIY" width="250"/>
  <img src="https://github.com/user-attachments/assets/1f2c79c9-1963-415e-b43d-945bbc477cae" alt="DIY" width="250"/>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/5d364985-a4ce-4b49-bf03-af7ce22bbc35" alt="20250614_195218" width="250"/>
  <img src="https://github.com/user-attachments/assets/8e66a3d6-527b-4047-9f0c-fb7c9cb2490f" alt="Xiaozhi Device" width="250"/>
  <img src="https://github.com/user-attachments/assets/4aa49b9e-ab2c-4949-aefc-9d51ecf6ac40" alt="identify your model" width="250"/>
</p>


By request, this GitHub project provides a simple way to use Xiaozhi-based devices with ESPHome. These compact devices can serve as voice assistants integrated with Home Assistant.

## 🚀 Quick Start Guide

### Step-by-step Installation:

1. **Connect your device** to your computer via USB. Open [ESPHome Web](https://web.esphome.io), click “**+ NEW DEVICE**”, and follow the prompts to set it up and connect it to Wi-Fi.
2. In ESPHome Builder, **take over the newly discovered device**, edit the configuration, paste your [device code](devices) but keep the original device `name`. (You can customize the `friendly_name` as desired.)
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
* Waveshare ESP32-S3-Touch-LCD-1.85C
## 🛒 Where to Buy

Ball v1 & v2: https://vi.aliexpress.com/item/1005008627679270.html

Muma Box: https://vi.aliexpress.com/item/1005009043526078.html

Muma Horse: https://vi.aliexpress.com/item/1005008884232596.html

Puck: https://www.aliexpress.com/item/1005009016529496.html

Guition Taichi pi: https://vi.aliexpress.com/item/1005007420092928.html

Xingzhi Cube 1.54: https://www.aliexpress.com/item/1005008565082769.html

Breadboard: Look in devices/Breadboard: https://github.com/RealDeco/xiaozhi-esphome/tree/main/devices/Breadboard

Breadboard Mini: https://www.aliexpress.com/item/1005009448496585.html

Waveshare 2.06" OLED Wrist Watch: https://vi.aliexpress.com/item/1005009516438849.html

Waveshare ESP32-S3-Touch-LCD-1.85C: https://www.aliexpress.com/item/1005008634826817.html

## Accesories:

3D file of "Eggvenger" figure used to hold the Ball in image above, use 115% for v2 since it's larger than v1.
https://makerworld.com/en/models/1238732-eggvenger-superhero-egg-holder

3D file for Wireless charger stand for the Guition JC3636W518 display
https://makerworld.com/en/models/238543-wireless-charger-holder

Wireless charger for the Guition JC3636W518 display: https://vi.aliexpress.com/item/1005005066837741.html

Curled audio cable for Guition JC3636W518 display: https://vi.aliexpress.com/item/1005007061609551.html

---EOF

