# Xiaozhi Ball V2 ESPHome Notes

## Project Origin

The original code for this device is based on:

- Main project: `https://github.com/RealDeco/xiaozhi-esphome`
- Specific device source: `https://github.com/RealDeco/xiaozhi-esphome/tree/main/devices/Spotpear%20Balls`

This repository builds on that base with structural changes, fixes, and device-specific improvements.

## Overview

This project contains ESPHome configurations for a ball-style device with:

- ESP32-S3
- `240x240` round display
- RGB status LED
- I2S microphone
- Speaker with ES8311 codec
- Home Assistant Voice Assistant integration
- Local wake word or Home Assistant wake word
- Physical buttons and Night Mode
- Touchscreen support on the full hardware variant

The configuration is split into a common base plus overlays:

- [`ball_v2_base.yaml`](ball_v2_base.yaml): shared common logic
- [`ball_v2_lite.yaml`](ball_v2_lite.yaml): wrapper for hardware without touch or battery
- [`ball_v2_full_hw.yaml`](ball_v2_full_hw.yaml): extra hardware for the full variant
- [`ball_v2.yaml`](ball_v2.yaml): wrapper for the full variant, combining base + full overlay

This reduces duplication: audio, UI, wake word, Night Mode, and Home Assistant integration are usually changed only once in the base file.

Important:

- [`ball_v2_base.yaml`](ball_v2_base.yaml) is a shared package, not a standalone YAML meant to be flashed by itself.
- Always validate and compile from [`ball_v2_lite.yaml`](ball_v2_lite.yaml) or [`ball_v2.yaml`](ball_v2.yaml).

## Main Features

The device can:

- work as a Home Assistant voice satellite
- show different screens depending on the assistant state
- play a startup sound
- display assistant timers
- show standby information:
  - digital clock
  - temperature
  - humidity
  - next alarm
- enter `Night Mode`
- open a touchscreen settings screen on the full hardware variant

## Visual States

The screen changes according to the current state:

- `idle`
- `listening`
- `thinking`
- `replying`
- `muted`
- `error`
- `no_wifi`
- `no_ha`
- `timer_finished`
- `settings_page` on the touchscreen variant

In the current `idle` screen:

- the avatar is shown while speaking and for a few seconds after returning to standby
- afterwards, the screen switches to a clean panel with configurable background and text colors
- if a timer is active, the countdown temporarily replaces the next alarm

Defaults:

- idle panel background: black
- text color: white

## Controls

### Physical Button

Current behavior:

- short press: speak / stop listening / cancel timer alarm
- triple quick press: toggle `Night Mode`
- very long press: factory reset

### Touchscreen

On the full hardware variant:

- swipe left from `idle`: open the settings screen
- swipe right: return to the normal screen
- if there is no interaction for `5s`, the settings screen closes automatically

Inside the settings screen:

- left side: volume
- right side: brightness
- both are shown as vertical bars with percentages
- dragging up or down inside each bar adjusts the corresponding level

Outside the settings screen:

- single tap: same primary action as the physical button
- double tap: toggle `Mute`

### Night Mode

`Night Mode` can be activated:

- from Home Assistant
- with a triple quick press on the physical button

When active, it:

- turns off the screen
- turns off the LED
- stops any current playback or announcements
- turns off the amplifier (`speaker_enable`)

Notes:

- `Night Mode` no longer changes the `Startup sound` switch persistently
- it simply blocks the startup sound while Night Mode is active

## Hardware Variants

### Maintenance Structure

#### Common Base

Edit [`ball_v2_base.yaml`](ball_v2_base.yaml) for shared changes such as:

- voice assistant
- wake word
- scripts and automations
- Night Mode
- display
- idle dashboard
- status LED
- sound
- Home Assistant integration

#### Full Overlay

Edit [`ball_v2_full_hw.yaml`](ball_v2_full_hw.yaml) only for hardware-specific additions:

- battery
- touchscreen
- touch gestures and settings screen interaction
- sensors or controls that only exist on that board

#### Wrappers

Wrappers only define identity and assemble packages:

- [`ball_v2_lite.yaml`](ball_v2_lite.yaml)
- [`ball_v2.yaml`](ball_v2.yaml)

### Full Variant

Use [`ball_v2.yaml`](ball_v2.yaml) if your device has:

- touchscreen
- battery
- or if you want to keep compatibility with that hardware

### Lite Variant

Use [`ball_v2_lite.yaml`](ball_v2_lite.yaml) if your device does not have:

- touchscreen
- battery

This avoids:

- touch controller errors
- unnecessary ADC reads
- log spam from missing hardware

## Home Assistant Integration

### Data Shown in Idle

In standby, the device can show:

- clock
- temperature
- humidity
- next alarm

Home Assistant exposes switches to enable or disable each item:

- `Show Idle Clock`
- `Show Idle Temperature`
- `Show Idle Humidity`
- `Show Idle Next Alarm`

These switches only control visibility.

The data source is no longer read directly from global Home Assistant sensors. Instead, each device exposes three editable `text` entities:

- `Idle Temperature Text`
- `Idle Humidity Text`
- `Idle Next Alarm Text`

Home Assistant decides what to show on each ball and writes already formatted text into those entities.

## Recommended Option: Publish Text from Home Assistant

This avoids multiple balls accidentally sharing the same data.

The flow is:

1. each ball exposes its own `text` entities
2. Home Assistant formats the content
3. a script or automation updates the specific device

## Example Home Assistant Automation

Generic example for a specific ball using ESPHome `text` entities:

```yaml
alias: Update example ball idle
triggers:
  - trigger: homeassistant
    event: start
  - trigger: state
    entity_id:
      - sensor.example_temperature
      - sensor.example_humidity
      - sensor.example_next_alarm
actions:
  - delay: "00:00:10"

  - action: text.set_value
    target:
      entity_id: text.example_ball_idle_temperature_text
    data:
      value: >-
        {% set v = states('sensor.example_temperature') %}
        {{ '' if v in ['unknown', 'unavailable', 'none', ''] else '%.2f °C' | format(v | float) }}

  - action: text.set_value
    target:
      entity_id: text.example_ball_idle_humidity_text
    data:
      value: >-
        {% set v = states('sensor.example_humidity') %}
        {{ '' if v in ['unknown', 'unavailable', 'none', ''] else '%.0f %%' | format(v | float) }}

  - action: text.set_value
    target:
      entity_id: text.example_ball_idle_next_alarm_text
    data:
      value: >-
        {% set v = states('sensor.example_next_alarm') %}
        {{ '' if v in ['unknown', 'unavailable', 'none', ''] else as_timestamp(v) | timestamp_custom('%d/%m %H:%M', true) }}
mode: restart
```

## Idle Layout

The current `idle` panel shows:

- temperature at the top with a thermometer icon
- humidity below with a water-drop icon
- a larger centered clock
- if a timer is active, a countdown below the clock
- otherwise, the next alarm below the clock with a yellow bell icon
- on the full variant, battery percentage below the alarm with a battery icon

The idle fonts already include `°` and `º`.

## Useful YAML Options

```yaml
idle_dashboard_background_color: "000000"
idle_dashboard_text_color: "FFFFFF"
idle_avatar_hold_ms: "2500"
```

These control:

- idle panel background color
- idle panel text color
- how long the avatar remains visible before returning to the idle dashboard

## Validation

To validate:

```bash
docker exec -it esphome esphome config ball_v2.yaml
docker exec -it esphome esphome config ball_v2_lite.yaml
```

To compile and upload:

```bash
docker exec -it esphome esphome run ball_v2.yaml
docker exec -it esphome esphome run ball_v2_lite.yaml
```

## Known Issues

### Strapping Pins

The log may warn about:

- `GPIO45`
- `GPIO46`

These are strapping pins. That is not automatically an error, but they must be used carefully because they affect ESP32-S3 boot behavior.

### Audio Race Conditions at Boot

There can still be interaction between:

- startup sound
- wake word
- I2S microphone initialization

Wake word startup has already been hardened so it does not begin while the `media_player` is still busy, but this remains a sensitive area.

### Remote Assets at Build Time

Images and sounds are downloaded at build time from the upstream project and embedded in the firmware.
