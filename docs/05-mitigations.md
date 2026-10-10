# 05 — Mitigations & Defensive Takeaways

POCSAG's lack of authentication is a **protocol property**, so most mitigations live *above* the RF layer or in operational process. The guiding principle:

> **A received page is an unauthenticated hint, not a trusted command.**

## For operators of paging networks

### 1. Never let a page alone trigger a critical action
Any safety-critical or irreversible response (evacuation, shutdown, medication, dispatch of force) must require **out-of-band confirmation** — a callback, a second channel, or a human verification step. This neutralises spoofed pages regardless of the RF weakness.

### 2. Add an application-layer integrity envelope
Where pagers cannot be replaced, wrap the message content in a **signed or HMAC-protected payload** that the receiving application (or a human following a codebook) can validate. A forged frame then fails validation even though the RF layer accepted it. This requires endpoint logic but needs no protocol change.

### 3. Migrate critical alerting to modern transports
For new or refreshable deployments, move safety-critical alerting to **encrypted, mutually authenticated** channels (secure cellular/data messaging, authenticated LMR, or purpose-built critical-alerting platforms). Keep paging only for non-authoritative, low-consequence notifications.

### 4. Monitor your own spectrum
Deploy an SDR-based monitor on your paging channels to detect:
- messages that never originated from your dispatch system,
- unexpected RICs/capcodes,
- anomalous transmission timing or power.
Rogue-transmitter detection turns the openness of the channel into a defensive advantage.

### 5. Reduce the attack surface
- Document every legitimate RIC/capcode and alert on anything outside that set.
- Retire unused pagers and decommission dormant channels.
- Educate staff that pager content can be forged.

## For red teams & assessors

- **Scope RF explicitly.** Healthcare, emergency-services, and OT/ICS engagements should include paging/RF in scope with written authorisation.
- **Test the *process*, not just the signal.** The highest-value finding is usually "a spoofed page caused a real operational response," which validates or breaks mitigation #1.
- **Operate lawfully.** Shielded environment or licensed spectrum only; never touch live operational channels. See [`../DISCLAIMER.md`](../DISCLAIMER.md).

## For regulators & standards bodies

- Treat broadcast paging in safety-critical roles as **deprecated-by-risk**; require compensating controls or migration timelines.
- Encourage authenticated-alerting standards for critical infrastructure.

---

## Summary table

| Mitigation | Layer | Effort | Residual risk addressed |
|-----------|-------|--------|-------------------------|
| Out-of-band confirmation | Process | Low | Spoofed critical actions |
| Signed/HMAC payload | Application | Medium | Forgery & tampering |
| Encrypted transport migration | Infrastructure | High | Confidentiality + authenticity |
| Spectrum monitoring | Detection | Medium | Active rogue transmitters |
| RIC hygiene & staff training | Operational | Low | Attack surface & human trust |
