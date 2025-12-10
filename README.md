# Ai Thinker ESP32 A1S Audio Kit board definition for `esp-adf`

## Usage

* Clone the repository by `git clone https://github.com/trombik/esp-adf-component-ai-thinker-esp32-a1s.git`
* In the root of your project directory create a `components`
* Create a symlink to
  `idf-component-ai-thinker-esp32-a1s/components/ai-thinker-esp32-a1s` under
  `components`.
* Run `idf.py menuconfig`
* Under `Audio HAL` > `Audio board` select `Custom audio board` 
* Back in main menu, under `Custom Audio Board` select correct version (see Supported boards below)
* Make sure to press `s` to save

## Supported boards

There are two different Ai Thinker A1S chips in the market: one with `ac101`
audio codec, another with `es8388`. Both chips has the same name. The one with
`ac101` is EoLed, the vendor claims. In addition, there are different Ai
Thinker A1S Audio Kit boards in the market: A1S `es8388` chips with different
pin assignments.

* Ai Thinker A1S with `es8388` variant 5 (most popular on AliExpress)
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
- [x] SD card
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

To use on-board microphones you need to resolder two capacitors to a correct position
To test on-board microphones, use `${ADF_PATH}/examples/recorder/pipeline_raw_http`.

See also "Known issues" below.

## SD card

`idf.py menuconfig` and under `Custom Audio Board` check `Use on-board SD card`. Press `s` to save.
SD card won't work unless you set `DATA3` and `CMD` on-board switches to the ON position. Otherwise you'll get errors about missing pull-up resistor.

## Battery charger

The battery charger should work, but no GPIO pin is connected to the charger
circuit. As such, the battery service in `esp-adf` cannot be used.

## Tested environment

- `esp-idf` `v5.0.2`, `v5.4-dirty`
- `esp-adf` `master` (last tested Dec 2025)

## Known issues

Some boards sold on AliExpress have hardware bug Audio. Two capacitors must be moved or added for on-board microphones to work. See
[a post](https://community.home-assistant.io/t/esp32-a1s-audio-kit-for-voice-assistant/568301/18) in
Home Assistant forum.

## Resources

* [The ‘AI Thinker Audio Kit' Arduino Challenge](https://www.pschatzmann.ch/home/2021/12/06/the-ai-thinker-audio-kit-experience-or-nothing-is-right/)
  A series of posts about Ai Thinker ESP32 A1S Audio Kit
* [Schematic](https://web.archive.org/web/20241216144927/https://docs.ai-thinker.com/_media/esp32-audio-kit_v2.2_sch.pdf)
* [ES8388 User Guide](https://dl.radxa.com/rock2/docs/hw/ds/ES8388%20user%20Guide.pdf)
* [Arduino driver](https://github.com/pschatzmann/arduino-audio-driver)
