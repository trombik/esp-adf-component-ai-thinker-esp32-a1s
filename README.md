# Ai Thinker ESP32 A1S Audio Kit board definition for `esp-adf`

## Usage

* Select `Custom audio board` under `Audio HAL` > `Audio board` in `menuconfig`
* Select a custom board under `Custom Audio Board`

## Supported boards

There are two different Ai Thinker A1S chips in the market: one with `ac101`
audio codec, another with `es8388`. Both chips has the same name. The one with
`ac101` is EoLed, the vendor claims. In addition, there are different Ai
Thinker A1S Audio Kit boards in the market: A1S `es8388` chips with different
pin assignments.

* Ai Thinker A1S with `es8388` (probably the latest variant available in
  AliExpress)

## What works

Here is a list of tested peripherals:

- [x] Audio output to speakers
- [ ] Keys
- [ ] On-board green LED
- [ ] Mic input
- [ ] Line input
- [ ] On-board microphones
- [ ] SD card
- [ ] Battery charger

## Tested environment

- `esp-idf` `v5.0.2`
- `esp-adf` `master`

## Known issues

The Audio Kit board has hardware bugs. Two capacitors must be connected to on-board microphones. See
[a post](https://community.home-assistant.io/t/esp32-a1s-audio-kit-for-voice-assistant/568301/18) in
Home Assistant forum.
