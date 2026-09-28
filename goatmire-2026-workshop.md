---
title: Goatmire 2026 AtomVM Workshop
---

# Goatmire 2026 AtomVM Workshop

This page collects the material for the **AtomVM** workshop at [Goatmire 2026](https://goatmire.com/).

During the workshop we write Elixir applications that run directly on the Goatmire badge, an ESP32-S3 board, using [AtomVM](https://github.com/atomvm/AtomVM).

## Slides

- [Workshop slides](slides/AtomVM-Goatmire-2026-Workshop.pdf)

## Workshop examples

All exercises are in this repository:

- [bettio/atomvm_workshop_examples](https://github.com/bettio/atomvm_workshop_examples/)

Each exercise is a standalone Mix project that runs on the badge. They build on each other, so it is best to follow them in order:

- [`01_hello_world`](https://github.com/bettio/atomvm_workshop_examples/tree/main/01_hello_world)
  Install AtomVM on the badge, then build, flash, and see console output from a first application.

- [`02_temperature`](https://github.com/bettio/atomvm_workshop_examples/tree/main/02_temperature)
  Talk to the TMP103 temperature sensor over I²C and print the temperature on the console.

- [`03_atomgl`](https://github.com/bettio/atomvm_workshop_examples/tree/main/03_atomgl)
  Show the temperature on the badge display with AtomGL.

- [`04_wifi_mqtt`](https://github.com/bettio/atomvm_workshop_examples/tree/main/04_wifi_mqtt)
  Join a Wi-Fi network, connect to an MQTT broker, and publish the temperature.

- [`05_neopixel`](https://github.com/bettio/atomvm_workshop_examples/tree/main/05_neopixel)
  Animate the badge's four SK6812MINI-E RGB LEDs, using the SPI peripheral as a waveform generator.

- [`06_keyboard`](https://github.com/bettio/atomvm_workshop_examples/tree/main/06_keyboard)
  Turn the 6 × 13 key matrix into press and release events, using interrupts instead of polling.

- [`07_analog`](https://github.com/bettio/atomvm_workshop_examples/tree/main/07_analog)
  Measure the battery voltage with the ESP32-S3 ADC.

## Getting started

You need Elixir installed on your computer. The `mix atomvm.*` tasks bring their own `esptool`, so nothing else has to be installed system-wide.

The first exercise installs AtomVM on the badge (this is needed only once):

```sh
cd 01_hello_world
mix deps.get
mix atomvm.esp32.install --image AtomVM-esp32s3-atomgl-ipv6-libsodium-psram-elixir-nightly-0.7
```

Then flash the application and watch its output:

```sh
mix atomvm.esp32.flash
mix atomvm.esp32.monitor --timeout 10
```

If the badge is not detected: hold **BOOT**, tap **RESET**, release **BOOT**, and retry.

## Hardware

The Goatmire badge is built around an **ESP32-S3-MINI-1** module and includes:

- a display with resistive touch panel,
- a TMP103 temperature sensor and an SC7A20 accelerometer on I²C,
- a Qwiic connector on the same I²C bus,
- four SK6812MINI-E addressable RGB LEDs,
- a 6 × 13 keyboard matrix,
- an IR transmitter and receiver,
- and battery and USB voltage sensing.

Pinout and more details are in the badge hardware notes:

- [`hardware/goatmire_badge.md`](https://github.com/bettio/atomvm_workshop_examples/blob/main/hardware/goatmire_badge.md)

### Datasheets

Microcontroller:

- [ESP32-S3-MINI-1 / MINI-1U Datasheet — Espressif](https://documentation.espressif.com/esp32-s3-mini-1_mini-1u_datasheet_en.pdf)
- [ESP32-S3 Datasheet — Espressif](https://documentation.espressif.com/esp32_s3_datasheet_en.pdf)

Temperature sensor:

- [TMP103 Datasheet — Texas Instruments](https://www.ti.com/lit/gpn/TMP103)
- [TMP103 Product Page — Texas Instruments](https://www.ti.com/product/TMP103)

Display controller:

- [ST7789 Product Family — Sitronix](https://www.sitronix.com.tw/en/products/aiot-device-ddi/)

RGB LEDs:

- [SK6812MINI-E Datasheet — Normand](https://www.normandled.com/upload/202004/SK6812MINI-E%20LED%20Datasheet.pdf)
- [SK6812MINI-E Product Page — Normand](https://www.normandled.com/Product/view/id/875.html)

Accelerometer:

- [SC7A20 Product Page / Datasheet — Silan](https://www.silan.com.cn/en/product/details/47.html)

## AtomVM documentation

The workshop uses AtomVM 0.7, so these links point to the release-0.7 documentation:

- [AtomVM release-0.7 documentation](https://doc.atomvm.org/release-0.7/)
- [AtomVM Tooling / ExAtomVM](https://doc.atomvm.org/release-0.7/atomvm-tooling.html#exatomvm)
- [Programmer's Guide](https://doc.atomvm.org/release-0.7/programmers-guide.html)

Peripherals:

- [I²C documentation](https://doc.atomvm.org/release-0.7/programmers-guide.html#i2c)
- [SPI documentation](https://doc.atomvm.org/release-0.7/programmers-guide.html#spi)
- [GPIO documentation](https://doc.atomvm.org/release-0.7/programmers-guide.html#gpio)
- [ESP32 ADC documentation](https://doc.atomvm.org/release-0.7/programmers-guide.html#esp32-adc)

Networking:

- [ESP32 Network API](https://doc.atomvm.org/release-0.7/programmers-guide.html#network-esp32-only)
- [Network Programming Guide](https://doc.atomvm.org/release-0.7/network-programming-guide.html)

Differences from the BEAM:

- [Differences between AtomVM and BEAM](https://doc.atomvm.org/release-0.7/differences-with-beam.html)
- [Stubbed Functions](https://doc.atomvm.org/release-0.7/stubbed-functions.html)

## Related resources

- [AtomVM](https://github.com/atomvm/AtomVM): a tiny Erlang VM for microcontrollers.
- [ExAtomVM](https://github.com/atomvm/exatomvm): the Mix tasks used to build and flash AtomVM applications.
