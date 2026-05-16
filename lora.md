---
title: LoRa, AtomVM, and Meshtastic experiments
---

# LoRa, AtomVM, and Meshtastic experiments

This page collects notes and resources around my experiments with **LoRa**, **AtomVM**, and **Meshtastic**.

## Starting with LoRa and AtomVM

A good entry point is this SX126x LoRa driver:

- [`lora_sx126x.erl`](https://github.com/bettio/pocketOS/blob/main/src/lora_sx126x.erl)

This is a fairly tested Erlang driver for Semtech SX126x LoRa chipsets that works with [AtomVM](https://github.com/atomvm/AtomVM). It provides LoRa communication from Erlang code running directly on microcontrollers such as the ESP32.

The driver is useful if you want to experiment with LoRa at a low level, without immediately depending on LoRaWAN or a higher-level mesh protocol.

Related reading:

- [AtomVM](https://github.com/atomvm/AtomVM): a tiny Erlang VM for microcontrollers.
- [Evaluating AtomVM for Fault-Tolerant ESP32-Based Systems](https://dl.acm.org/doi/10.1145/3759161.3763048): a paper evaluating AtomVM on ESP32-based systems, including LoRa-based experiments.

## Meshtastic experiments

I also implemented part of the [Meshtastic](https://meshtastic.org/) protocol from scratch in Erlang.

Meshtastic is an open source, off-grid mesh communication system that uses inexpensive LoRa radios to exchange messages without relying on internet or cellular infrastructure.

My Erlang implementation runs on AtomVM and is used in the `pocketOS` project.

### Source code

- [`meshtastic.erl`](https://github.com/bettio/pocketOS/blob/main/src/meshtastic.erl)
  Packet parsing, serialization, channel hash calculation, and AES-CTR encryption/decryption helpers.

- [`meshtastic_server.erl`](https://github.com/bettio/pocketOS/blob/main/src/meshtastic_server.erl)
  A small server process that handles incoming Meshtastic packets, duplicate detection, rebroadcasting, node identity, and outgoing messages.

- [`meshtastic_proto.erl`](https://github.com/bettio/pocketOS/blob/main/src/meshtastic_proto.erl)
  Minimal protobuf encoding/decoding support for selected Meshtastic payloads such as text messages, position, node info, and telemetry.

Useful upstream Meshtastic references:

- [Meshtastic introduction](https://meshtastic.org/docs/introduction/)
- [How Meshtastic works](https://meshtastic.org/docs/getting-started/)
- [Meshtastic mesh broadcast algorithm](https://meshtastic.org/docs/overview/mesh-algo/)
- [Meshtastic documentation](https://meshtastic.org/docs/)

## pocketOS

These experiments are part of [pocketOS](https://github.com/bettio/pocketOS), an Elixir firmware/application for ESP32-based handheld devices running on AtomVM.

pocketOS turns small handheld boards into hackable cyberdecks with a BEAM-based software stack. Among other things, it includes:

- a native Elixir UI,
- an interactive Lisp environment,
- GPS and mapping experiments,
- LoRa communication,
- and a Meshtastic-compatible messaging layer.

The Meshtastic implementation above is used by pocketOS to send messages over LoRa without depending on existing infrastructure.

## Hardware

The experiments target ESP32-based handheld devices with LoRa radios, especially:

- [LILYGO T-Deck](https://lilygo.cc/products/t-deck)
  A compact ESP32-S3 handheld with screen, keyboard, trackball, and LoRa variants.

- [LILYGO T-Lora Pager](https://lilygo.cc/products/t-lora-pager)
  A pager-style ESP32-S3 handheld with keyboard, display, foldable antenna, and LoRa variants.

Meshtastic also has device-specific pages for these boards:

- [Meshtastic: LILYGO T-Deck](https://meshtastic.org/docs/hardware/devices/lilygo/tdeck/)
- [Meshtastic: LILYGO T-Lora Pager](https://meshtastic.org/docs/hardware/devices/lilygo/tpager/)

## Repository links

Main project:

- [bettio/pocketOS](https://github.com/bettio/pocketOS)

LoRa and Meshtastic-related files:

- [`src/lora_sx126x.erl`](https://github.com/bettio/pocketOS/blob/main/src/lora_sx126x.erl)
- [`src/meshtastic.erl`](https://github.com/bettio/pocketOS/blob/main/src/meshtastic.erl)
- [`src/meshtastic_server.erl`](https://github.com/bettio/pocketOS/blob/main/src/meshtastic_server.erl)
- [`src/meshtastic_proto.erl`](https://github.com/bettio/pocketOS/blob/main/src/meshtastic_proto.erl)
