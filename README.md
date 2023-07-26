# Ai Thinker ESP32 A1S Audio Kit board definition for `esp-adf`

## Usage

* Clone the repository by `git clone https://github.com/trombik/esp-adf-component-ai-thinker-esp32-a1s.git`
* Create `components` directory in a project directory
* Create a symlink to
  `idf-component-ai-thinker-esp32-a1s/components/ai-thinker-esp32-a1s` under
  `components`.
* Run `idf.py menuconfig`
* Select `Custom audio board` under `Audio HAL` > `Audio board`
* Select a custom board under `Custom Audio Board`

## Supported boards

There are two different Ai Thinker A1S chips in the market: one with `ac101`
audio codec, another with `es8388`. Both chips has the same name. The one with
`ac101` is EoLed, the vendor claims. In addition, there are different Ai
Thinker A1S Audio Kit boards in the market: A1S `es8388` chips with different
pin assignments.

* Ai Thinker A1S with `es8388` variant 5 (probably the latest variant available in
  AliExpress)
* Ai Thinker A1S with `es8388` variant 7

## What works

Here is a list of tested peripherals:

- [x] Audio output to speakers
- [x] Keys
- [x] On-board LEDs
- [x] Bluetooth
- [ ] Mic input
- [ ] Line input
- [x] On-board microphones
- [ ] SD card
- [x] Battery charger

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

According to the schematic,
[ADC Button Peripheral](https://docs.espressif.com/projects/esp-adf/en/latest/api-reference/peripherals/periph_adc_button.html)
is supposed to work. However, my board does not have necessary resistors
(`R60`, `R61` ...). If yours has them,
give `CONFIG_AI_THINKER_ESP32_A1S_ES8388_BUTTON_KEY_ADC` a try.

### On-board LED

The board has two red LEDs connected to GPIO pins. By default, only one LED is
useable. Unlike official development boards from `espressif`, the color is
red. `esp-adf` provides `get_blue_led_gpio()` and `get_green_led_gpio()`, not
one for red LED.

To test the LED, use `${ADF_PATH}/examples/checks/check_display_led`.

## Bluetooth

To test `a2dp` sink, use `${ADF_PATH}/examples/player/pipeline_a2dp_sink_stream`.

`a2dp` source has not been tested.

## On-board microphones

To test on-board microphones, use `${ADF_PATH}/examples/recorder/pipeline_raw_http`.

See also "Known issues" below.

## Battery charger

The battery charger should work, but no GPIO pin is connected to the charger
circuit. As such, the battery service in `esp-adf` cannot be used.

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
* [ES8388 User Guide](https://dl.radxa.com/rock2/docs/hw/ds/ES8388%20user%20Guide.pdf)
