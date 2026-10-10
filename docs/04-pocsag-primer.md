# 04 — POCSAG Primer: How It Works and Why It Is Insecure

**POCSAG** (Post Office Code Standardisation Advisory Group), standardised as **ITU-R M.584** / Radio Paging Code No. 1, is the most widely deployed paging protocol. It was designed in the early 1980s for one job: **delivering short messages reliably over a noisy one-way radio channel.** Security was never a design goal.

## Physical layer

| Property | Value |
|----------|-------|
| Modulation | 2-FSK (binary frequency-shift keying) |
| Bitrates | 512, 1200, or 2400 bps |
| Direction | One-way (base → pager); pagers do not transmit |
| Bands | VHF / UHF, commonly in the 137–160 and 440–470 MHz ranges |

## Frame structure

A POCSAG transmission is built from fixed-size 32-bit codewords:

```
┌───────────┬──────────┬──────────────────────────────────────────────┐
│ Preamble  │  Sync    │  Batch 1 → Batch N                             │
│ 576 bits  │ codeword │  each = 1 sync word + 8 frames × 2 codewords   │
│ 1010...   │ 0x7CD215 │                                                │
└───────────┴──────────┴──────────────────────────────────────────────┘
```

- **Preamble** — an alternating `1010…` pattern (≥576 bits) so receivers can achieve bit sync.
- **Synchronisation codeword** — the fixed value `0x7CD215`, marking the start of a batch.
- **Batches** — each contains 8 frames; each frame holds two 32-bit codewords.

### Codeword types

Each 32-bit codeword is one of two kinds, distinguished by its first bit:

- **Address codeword** (bit 0 = `0`) — carries 18 address bits plus 2 function bits. The pager's **RIC / capcode** determines which frame position its address must appear in.
- **Message codeword** (bit 0 = `1`) — carries 20 bits of payload (numeric BCD, or alphanumeric 7-bit characters packed across codewords).

### Error protection

Every codeword ends with a **BCH(31,21) code and one even-parity bit.** This can correct up to 2 bit errors and detect more.

## The root of the insecurity

The BCH code is the only "protection" in the frame — and it protects against **noise, not adversaries.** Note what is absent:

| Security property | Present in POCSAG? |
|-------------------|--------------------|
| Confidentiality (encryption) | ❌ None — plaintext |
| Sender authentication | ❌ None |
| Message integrity vs. tampering | ❌ None (ECC ≠ MAC) |
| Replay protection | ❌ None |
| Key material / shared secret | ❌ None |

Because the ECC is a *public, deterministic* function, anyone can compute valid codewords. A receiver tuned to the right frequency and bitrate will accept **any** well-formed frame addressed to a RIC it listens for — it has no mechanism to ask "who sent this?" or "has this been altered?"

### Consequence

Message injection, spoofing, and replay are **inherent** to the protocol, not bugs in a particular pager. The barrier to abuse is operational (hardware, spectrum access, legality) — not cryptographic. That is precisely why safety-critical use of paging is risky, and why the mitigations in [`05-mitigations.md`](05-mitigations.md) focus on **not trusting the message by itself.**
