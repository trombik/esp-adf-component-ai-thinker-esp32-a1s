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
- [x] Keys
- [x] On-board LEDs
- [ ] Mic input
- [ ] Line input
- [ ] On-board microphones
- [ ] SD card
- [ ] Battery charger

### Speaker outputs

To test speaker outputs, use `${ADF_PATH}/examples/player/pipeline_http_mp3`.

### Keys

To test keys, use `${ADF_PATH}/examples/checks/check_board_buttons`.

| Name   | Function in `esp-adf` |
|--------|-----------------------|
| `KEY1` | Mode                  |
| `KEY2` | Rec                   |
| `KEY3` | Play                  |
| `KEY4` | Set                   |
| `KEY5` | Volume down           |
| `KEY6` | Volume up             |

Note that `KEY2` cannot be used with SD card because the `GPIO` for `KEY2` is
shared with SD card peripheral.

### On-board LED

The board has two red LEDs connected to GPIO pins. By default, only one LED is
useable. Unlike official development boards from `espressif`, the color is
red. `esp-adf` provides `get_blue_led_gpio()` and `get_green_led_gpio()`, not
one for red LED.

To test the LED, use `${ADF_PATH}/examples/checks/check_display_led`.

## Tested environment

- `esp-idf` `v5.0.2`
- `esp-adf` `master`

## Known issues

The Audio Kit board has hardware bugs. Two capacitors must be connected to on-board microphones. See
[a post](https://community.home-assistant.io/t/esp32-a1s-audio-kit-for-voice-assistant/568301/18) in
Home Assistant forum.

## Resources

* [The‘AI Thinker Audio Kit' Arduino Challenge](https://www.pschatzmann.ch/home/2021/12/06/the-ai-thinker-audio-kit-experience-or-nothing-is-right/)
  A series of posts about Ai Thinker ESP32 A1S Audio Kit
* [Schematic](https://docs.ai-thinker.com/_media/esp32-audio-kit_v2.2_sch.pdf)
* [arduino-audiokit/tree/main/src/audio_board](https://github.com/pschatzmann/arduino-audiokit/tree/main/src/audio_board)
  An implementation for Arduino
