# 02 — Hardware & Firmware Setup

## Overview

The experiment uses two independent SDR platforms so that transmission and reception are physically separate — proving that a message crafted on one device is accepted by another with no shared state, no pairing, and no authentication.

```
  ┌───────────────────────────┐          UHF RF           ┌──────────────────────────┐
  │  HackRF One + PortaPack    │  ~~~~~~~~~~~~~~~~~~~~~~~>  │  Flipper Zero             │
  │  H2+  (Mayhem firmware)    │     POCSAG 2-FSK frame    │  (RogueMaster firmware)   │
  │  — TRANSMIT                │     512 / 1200 bps        │  — RECEIVE / RECON        │
  └───────────────────────────┘                           └──────────────────────────┘
        crafts & injects                                        scans, decodes, verifies
```

## Transmit platform — HackRF One + PortaPack H2+

| Item | Detail |
|------|--------|
| SDR | **HackRF One** — 1 MHz–6 GHz half-duplex transceiver |
| Add-on | **PortaPack H2+** — LCD, controls, and battery for standalone (no-PC) operation |
| Firmware | **Mayhem** — community firmware that adds a large suite of on-device applications, including a **POCSAG TX** module |
| Why | Provides a self-contained, menu-driven POCSAG transmitter. No host computer or GNU Radio flowgraph is required, which keeps the demonstration simple and reproducible. |

## Receive / reconnaissance platform — Flipper Zero

| Item | Detail |
|------|--------|
| Device | **Flipper Zero** — multi-tool with a sub-GHz transceiver |
| Firmware | **RogueMaster** — community firmware expanding sub-GHz capability, scanning, and decoding |
| Why | Acts as an independent receiver to confirm that an injected frame is received and decoded as a valid POCSAG message. Its band scanner is also used for the reconnaissance phase. |

> Note on range: the Flipper Zero's sub-GHz front end is optimised for the ISM bands. UHF paging reception around 430–450 MHz is at the edge of practical use and depends on firmware, antenna, and proximity. In this bench setup the two radios sit close together at low power, which is sufficient to demonstrate the signalling weakness.

## Antennas & power

- UHF-tuned whip antennas on both devices.
- **Minimum viable transmit power**, short bench distance only.
- The objective is a clean decode over centimetres, **not** range.

## Required operating environment (non-negotiable)

To keep any reproduction lawful and harmless:

- Operate inside a **shielded RF enclosure, dummy load, or Faraday environment**, or under a valid licence on authorised frequencies.
- Ensure **no third-party, operational, or safety pagers** are within range.
- Use **benign test payloads** only.

See [`../DISCLAIMER.md`](../DISCLAIMER.md).
