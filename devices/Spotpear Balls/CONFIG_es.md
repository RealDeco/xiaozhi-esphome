# Xiaozhi Ball V2 ESPHome Notes

## Project Origin

The original code for this project is based on:

- Main project: `https://github.com/RealDeco/xiaozhi-esphome`
- Specific device source: `https://github.com/RealDeco/xiaozhi-esphome/tree/main/devices/Spotpear%20Balls`

Based on that foundation, adaptations, fixes, and structural changes have been implemented for this repository.

## Overview

This project contains ESPHome configurations for a "ball" type device featuring:

- ESP32-S3
- `240x240` round display
- RGB status LED
- I2S Microphone
- Speaker with ES8311 codec
- Home Assistant Voice Assistant integration
- Local or Home Assistant-based wake word
- Physical buttons and Night Mode

The configuration is now separated into a common base + overlays:

- [`ball_v2_base.yaml`](ball_v2_base.yaml): All shared common logic.
- [`ball_v2_lite.yaml`](ball_v2_lite.yaml): Wrapper for hardware without touch or battery.
- [`ball_v2_full_hw.yaml`](ball_v2_full_hw.yaml): Hardware extras for the full version.
- [`ball_v2.yaml`](ball_v2.yaml): Wrapper for the full variant, loading both the common base and the full overlay.

This structure reduces duplication: if you change the audio, UI, wake word, night mode, or Home Assistant integration, you usually only do it once in the base file.

**Important:**

- [`ball_v2_base.yaml`](ball_v2_base.yaml) is a base package; it is not a standalone YAML for flashing or validation on its own.
- Always validate and compile from [`ball_v2_lite.yaml`](ball_v2_lite.yaml) or [`ball_v2.yaml`](ball_v2.yaml).

## Main Features

The device can:

- Act as a voice satellite for Home Assistant.
- Display different screens depending on the assistant's state.
- Play a startup sound and a wake word notification sound.
- Display assistant timers.
- Show standby information:
  - Digital clock
  - Temperature
  - Humidity
  - Next alarm
- Enter `Night Mode`.

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

In the current `idle` version:

- The avatar is displayed while speaking and for a few seconds after returning to standby.
- Afterwards, a clean panel is shown with configurable background and text colors.

By default:
- Idle panel background: Black
- Text color: White

## Controls

### Physical Button

Based on the current configuration:

- **Short press**: Speak / Stop listening / Cancel timer alarm.
- **Triple quick press**: Toggle `Night Mode`.
- **Very long press**: `Factory reset`.

### Night Mode

`Night Mode` can be activated:

- From Home Assistant.
- Via a triple quick press of the button.

When activated:

- Turns off the screen.
- Turns off the LED.
- Disables `wake_sound`.
- Disables `startup_sound_switch`.
- Stops any current playback/announcements.
- Turns off the amplifier (`speaker_enable`).

## Hardware Variants

### Maintenance Structure

#### Common Base
Edit [`ball_v2_base.yaml`](ball_v2_base.yaml) for shared changes such as:
- Voice assistant & Wake word
- Scripts and automations
- Night mode
- Display & Idle dashboard
- Status LED & Sound
- Home Assistant integration

#### Full Overlay
Edit [`ball_v2_full_hw.yaml`](ball_v2_full_hw.yaml) only for additional hardware:
- Battery
- Touchscreen
- Sensors or controls exclusive to that board.

#### Wrappers
Wrappers only define identity and assemble the packages:
- [`ball_v2_lite.yaml`](ball_v2_lite.yaml)
- [`ball_v2.yaml`](ball_v2.yaml)

### Full Variant
Use [`ball_v2.yaml`](ball_v2.yaml) if your device has:
- Touchscreen
- Battery
- Or if you wish to maintain compatibility with that hardware.

### Lite Variant
Use [`ball_v2_lite.yaml`](ball_v2_lite.yaml) if your device lacks:
- Touchscreen
- Battery

This prevents touch controller errors, unnecessary ADC readings, and log spam from missing hardware.

## Home Assistant Integration

### Data Displayed in Idle
In standby mode, the device can show:
- Clock
- Temperature
- Humidity
- Next alarm

Switches appear in Home Assistant to toggle each one:
- `Show Idle Clock`
- `Show Idle Temperature`
- `Show Idle Humidity`
- `Show Idle Next Alarm`

These switches only control visibility.

The data source is no longer read directly from global HA sensors. Instead, each device exposes three editable `text` entities:
- `Idle Temperature Text`
- `Idle Humidity Text`
- `Idle Next Alarm Text`

Home Assistant decides what to show on each device and writes the pre-formatted text to these entities.

## Recommended Option: Publishing Text from Home Assistant

This approach prevents multiple devices from accidentally sharing the same data.

The workflow is:
1. Each device exposes its own `text` entities.
2. Home Assistant formats the content.
3. A script or automation updates the specific device.

## Example Home Assistant Automation

Generic example for a specific device using the `text` entities exposed by ESPHome:

```yaml
alias: Update idle example ball
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
