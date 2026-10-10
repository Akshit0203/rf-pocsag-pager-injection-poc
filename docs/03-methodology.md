# 03 — Methodology

> Described at a conceptual level. This document does **not** provide exact payloads, operational frequencies, or target addresses — see [`../DISCLAIMER.md`](../DISCLAIMER.md).

The exercise follows a standard signals workflow: **reconnaissance → identification → crafting → controlled injection → verification → variant testing.** Each phase below lists its purpose, the tool used, and the control that keeps it lawful.

---

## Phase 1 — UHF Reconnaissance

- **Goal:** characterise activity in the UHF paging range (430–450 MHz) and confirm a POCSAG-bearing channel exists.
- **Tool:** Flipper Zero (RogueMaster) frequency analyser / scanner.
- **Observed:** periodic signal bursts consistent with paging traffic.
- **Evidence:** `assets/screenshots/demo-01-uhf-recon-scan.jpg`, `demo-02-recon-band-plan.jpg`.

## Phase 2 — Channel Identification

- **Goal:** isolate a candidate channel and note its modulation (2-FSK) and bitrate (512 / 1200 bps).
- **Tool:** Flipper Zero scanner + POCSAG frequency/channel list.
- **Evidence:** `assets/screenshots/demo-03-pocsag-frequency-list.jpg`.

## Phase 3 — Message Crafting

- **Goal:** compose a **benign test message** in the PortaPack POCSAG TX module.
- **Parameters set:** frequency, bitrate, address (RIC/capcode), and message body.
- **Control:** payloads are harmless test strings; no real/operational RIC is published.
- **Evidence:** `assets/screenshots/demo-04-message-crafting.jpg`.

## Phase 4 — Controlled Injection

- **Goal:** transmit the crafted frame to prove a non-network transmitter can emit an acceptable POCSAG message.
- **Tool:** HackRF One + PortaPack (Mayhem) POCSAG TX.
- **Control:** minimum power, isolated/shielded environment, no third-party receivers in range.
- **Evidence:** `assets/screenshots/demo-05-pocsag-tx-inject.jpg`.

## Phase 5 — Reception & Verification

- **Goal:** independently receive and decode the injected frame on a *different* radio, proving it was accepted with **no authentication challenge**.
- **Tool:** Flipper Zero (RogueMaster) decoder.
- **Result:** the test message decoded cleanly and matched the crafted content.
- **Evidence:** `assets/screenshots/demo-06-message-received.jpg`, `assets/video/demo.mp4`.

## Phase 6 — Variant Testing

- **Goal:** confirm repeatability across different message types (e.g. broadcast-style vs. individual-address test messages).
- **Result:** injection was immediate and reproducible on demand.

---

## Why this proves the vulnerability

The transmitter and receiver share **no secret, no pairing, and no prior state.** Despite that, a frame crafted on one device was accepted and displayed by the other. That is only possible because POCSAG performs **no sender authentication and no integrity verification** — the entire point of the demonstration. See [`04-pocsag-primer.md`](04-pocsag-primer.md).
