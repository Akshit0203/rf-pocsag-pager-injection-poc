# 01 — Background: The 2024 Lebanon Pager Incident

## Context

In September 2024, thousands of pagers — and subsequently a second wave of two-way radios — carried by members of an organisation in Lebanon and Syria were weaponised in a coordinated operation widely attributed in public reporting to Israeli intelligence. The incident caused dozens of deaths and thousands of injuries.

The physical mechanism of that operation (compromised device supply chains and concealed explosive payloads) is **outside the scope of this project** and is not studied, reproduced, or documented here.

## What this project *does* study

The incident drew global attention to a long-standing and separate question: **the trustworthiness of the paging networks that still underpin critical communications.** Pagers are not obsolete. They remain in daily operational use because they offer:

- **Long range** from a small number of high-power transmitters,
- **Excellent in-building and underground penetration** (low UHF/VHF frequencies),
- **Simplicity and battery life** that modern smartphones cannot match,
- **One-to-many broadcast** that is ideal for mass alerting.

Those same properties made paging indispensable in hospitals, emergency services, and industrial sites — and they are exactly why the networks are hard to retire.

## The core problem

The dominant paging protocol, **POCSAG**, was standardised in the early 1980s (ITU-R M.584). It was designed for *reliable delivery over noisy channels*, not for *security*. It has:

- **no encryption** — messages travel in plaintext,
- **no sender authentication** — a receiver cannot verify who transmitted,
- **no integrity or anti-replay protection** beyond an error-correcting code meant only to survive RF noise.

This means any transmitter on the correct frequency, bitrate, and address can emit a frame that in-range pagers will accept and display as genuine. That is a *protocol-level* property, not a device defect — and it is the property this project demonstrates in a controlled environment.

## Why document it

Making this risk legible to **defenders, operators, and regulators** is the entire purpose of this repository. Healthcare, emergency, and OT/ICS environments that still depend on paging should understand that a received page carries no inherent proof of authenticity — and should plan their safety-critical workflows accordingly.

See [`04-pocsag-primer.md`](04-pocsag-primer.md) for the protocol-level detail and [`05-mitigations.md`](05-mitigations.md) for defensive guidance.
