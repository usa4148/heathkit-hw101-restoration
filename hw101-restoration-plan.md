---
title: "Heathkit HW-101 Transceiver — Restoration Plan"
subtitle: "Rev. 1 — field sources, Heath service bulletins, W5RKL and Latta notes (manual cross-check pending)"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
toc: true
---

<!-- Build: pandoc hw101-restoration-plan.md -o hw101-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **Safety.** The HW-101 runs on **~700–800 V DC** plate supply from the HP-23 power supply. That voltage stays on the filter capacitors after the supply is unplugged. Discharge with a grounded bleeder stick before every hands-in session, and never operate the final cage with its cover off. Work one-handed when the set is live.

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath HW-101 Assembly & Operation manual** (245 pp.) | Bill Latta's HW-101 site, frostburg.edu | Primary reference: parts lists, voltage charts, alignment, schematic. *Scan only; cross-check pending (§11).* |
| **HW-101 Service Bulletins**, HW-101-1 … -85 (1971–79) | Latta site; text of -1 to -55 on the Heath list (Jul 1997) | Factory fixes. The bulletin numbers below come from these. |
| **HP-23 power supply manual** | Latta site | Required companion supply. |
| **W5RKL**, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting* | w5rkl.com | First power-up checklist, bias/neutralizing, alignment checks. |
| **Latta (Frostburg) HW-101 restoration, alignment, HP-23 restoration pages** | frostburg.edu | Documented restoration with measured failures. |
| **WB8JKR**, *Optimizing the HW-101 / SB-100–102* | mikeyancey.com | Meter-zero, carrier-crystal, receive/transmit improvements. |
| **K4JPN**, *Improve Your HW-101*; **KM5Z** refurb notes | nostalgickitscentral.com, pestingers.net | Tube-brand problems, T/R relay, pilot lamps. |
| Heath list archive / Heathkit-Radios (groups.io) | puck.nether.net, groups.io | Field experience. |

# 2. Transceiver at a glance

## 2.1 Key facts

| Item | Value |
|---|---|
| Built | 1970–1983; 30,000–40,000 kits. Nearly identical to the SB-102. |
| Bands | 80, 40, 20, 15, 10 m (eight 500 kHz segments, 3.5–29.5 MHz) |
| Modes | SSB (USB/LSB), CW |
| IF | 3395 kHz; 1st conversion 8.395–8.895 MHz bandpass (T202) |
| Filters | 2.1 kHz SSB (supplied); optional 400 Hz CW, front-panel switched |
| Power | 180 W PEP input SSB, 170 W CW; about 100 W out (80 W on 10 m) |
| VFO | Solid-state: **MPF-105 FET** oscillator, **2N3393** (Q942) regulator, V20 6AU6 buffer |
| Supply | External **HP-23** (A/B/C) or PS-23 (AC), HP-13 (mobile) |
| Pilot lamps | **#47** (part of the 12.6 V filament string balance; bulletin HW-101-20) |

## 2.2 Tube complement

The V numbers below are assembled from the field sources and bulletins. **Verify them against the manual's tube chart before ordering.**

| V | Tube | Function | Known issues (source) |
|----|----------|--------------------|----------------------------------------------|
| V1 | 6EA8 | Speech amp / cathode follower | GE 6EA8s oscillate (~65 kHz) → VOX chatter, no carrier null (HW-101-21, -31). 6GH8A/6CQ8 are used as substitutes. |
| V2 | 6AU6 | Carrier isolation amp | — |
| V3 | 6AU6 | 1st IF amp | Leakage → S-meter drift (HW-101-34). El-Menco brand draws excess current → can't zero meter. |
| V4 | 6AU6 | 2nd IF amp | Brand-sensitive: GE / IEC OK; some Sylvania/RCA/Westinghouse didn't work (HW-101-46). |
| V5 | 6EA8 | 1st transmit mixer | Check seating. |
| V6 | 6CB6 | 2nd transmit mixer | Under shield; shield contact matters. |
| V7 | 6CL6 | Driver | Under shield. |
| V8, V9 | **6146A / 6146B** | Finals (class AB1) | 1978: Heath said use 6146A only (HW-101-18, -39). 1979: GE 6146B approved (HW-101-49). **Re-neutralize after any change.** |
| V10 | 6HS6 | Receiver RF amp | **High failure rate.** Leakage or shorts → poor AGC, fast S-meter decay, lost sensitivity, "static" on S-meter (HW-101-24). Run at or above ratings. A 6AU6 is a drop-in substitute. |
| V11 | 6HS6 | 1st receive mixer | Same as V10. Some 6HS6s had swapped pinouts (HW-101-12). |
| V12 | 6EA8 | 2nd receive mixer / relay amp | Defective (IEC) tubes hold relays in after TX (HW-101-23); El-Menco may oscillate (HW-101-17). |
| V13 | 6BN8 | Product detector / AVC | Weak 6BN8s found on test. |
| V14 | 6GW8 | Audio amp / output | Runs hot. |
| V15 | 6EA8 | CW sidetone osc / amp | Shorts → sidetone problems or continuous keying (HW-101-74). A tube shield cures bleed-through. |
| V16 | 12AU7 | Carrier (sideband) oscillator | Plate resistors R6/R7 drift (§3.2). |
| V17 | 12AT7 | VOX amp / 100 kHz calibrator | — |
| V18 | 0A2 | 150 V regulator (VFO) | Flicker on CW points to an HP-23 on the 250 V tap or a bad C5. |
| V19 | 12AT7 | Heterodyne osc / cathode follower | Repeated failure: change R212 (HW-101-2). |
| V20 | 6AU6 | VFO buffer amp | Feeds the FET's ~8 V regulator; a poor tube → drift. El-Menco → CW chirp. |

**Brand note (K4JPN, W5RKL):** El-Menco 6AU6, 6HS6, 6EA8, 12AT7, 12AU7 and 6CL6 tubes are repeatedly implicated. Keep spares of other brands.

## 2.3 Semiconductors

The HW-101 has 6 germanium diodes, 9 silicon diodes, 1 zener, the MPF-105 FET and the 2N3393. Named in the bulletins:

- **CR1–CR4** (modulator): germanium; bulletin HW-101-13 upgraded them to hot-carrier diodes (56-87).
- **CR201** (calibrator), **CR901** (relative power), **CR941**: germanium 1N191-type (56-26).
- **D201**: VOX. Leakage here kills VOX sensitivity (HW-101-6).
- **D907**: orientation affects 15/10 m receive sensitivity (HW-101-11).

# 3. What fails, ranked

## 3.1 Capacitors

**Electrolytics in the transceiver** (Latta's measured list; W5RKL gives 450 V/50 V ratings for the SB versions):

| Ref | Value | Role | Replace with |
|---|---|---|---|
| C2 | 10 µF 15 V | Speech amp cathode bypass (modulator board) | 10 µF 50 V |
| C12 | 20 µF 350 V | Speech amp B+ decoupling | 22 µF 450 V |
| C212 | 10 µF 15 V | VOX amp cathode bypass (bandpass board) | 10 µF 50 V |
| C304 | 20 µF 350 V | Audio output B+ decoupling (audio board) | 22 µF 450 V |
| C906 | 10 µF 150 V | Bias filter | 10 µF 250 V; **negative supply, so check polarity** |

Keep the same capacitance in audio and speech circuits so the frequency response doesn't change. A higher voltage rating is fine. The main filter capacitors are in the HP-23 (§3.6).

| Other capacitor | Likelihood | Symptom | Action |
|---|---|---|---|
| C946 (4700 pF, VFO) | Medium | Warble when tapped | Glue its top to the VFO wall (HW-101-42). |
| C947 (56 pF, VFO shift) | Medium | Intermittent USB/LSB shift | 47 pF NPO (HW-101-19). |
| C213 (.2 µF, VOX delay) | Medium | Short VOX hold, VOX trouble | Leakage-test; .47 µF (HW-101-63). |
| C701, C801 (680 pF 300 V) | Medium | R940 overheats | 680 pF 500 V (HW-101-75). |
| Silver micas in carrier/BFO (C17 etc.) | Low–medium | Weak or uneven carrier | Test; WB8JKR changes C17 from 12 pF to 100 pF. |
| Ceramic discs | Low | — | Spot-test bypasses. |

## 3.2 Resistors

These are 1/2–2 W carbon composition parts, ~50 years old. They drift high, worst where they run hot.

| Ref | Value | Failure / symptom | Fix (source) |
|---|---|---|---|
| **R106** | 22 kΩ 1 W (IF board) | Heats and changes → **S-meter zero drift** | 22 kΩ 2 W wirewound (WB8JKR); K4JPN uses 33 kΩ 2 W. Four other connections share its pad, so keep them all. |
| **R107** | 100 kΩ 1/2 W | S-meter drift with heat | 100 kΩ 1 W (HW-101-10). |
| **R6, R7** | 33 kΩ (carrier osc V16 plates) | One measured **330 kΩ**, another 42 kΩ → low USB/CW gain and output | Replace with 1 W (HW-101-84); WB8JKR uses 27 kΩ to raise carrier level. |
| **R1** | 100 kΩ (speech amp) | Heat damage | 1 W (HW-101-84); also check R215 100 kΩ and R316 22 kΩ. |
| **R117** | 3.3 kΩ (AVC) | Open → AVC and S-meter decay too fast | Replace; route the AVC wire ends away from it (HW-101-55). |
| **R915** | 3.3 MΩ | Open → no USB/LSB output (TUNE OK), ALC upset | Replace (HW-101-69). |
| **R940, R904** | 100 Ω | Overheat or short to the driver shield | Sleeve the leads, dress away from the shield (HW-101-37, -50). |
| **R701** | 1 kΩ (driver grid switch board) | Burned, from a bandswitch lug shorting to the shield cover | Replace and bend the lug clear (W5RKL). |
| **R212** | 220 Ω | Repeated heterodyne-oscillator tube failure | 330 Ω 1/2 W (HW-101-2). |
| R945, R947 | VFO | Low VCO output; VFO stops at the high end | Per HW-101-52, -80 (§4.6). |
| **R202** | 10 kΩ | *Early units only.* Limits drive to V5 | Jumper it (HW-101-14). Latta shunted it with wire to keep the early-run marker. |
| 10 kΩ plate-supply resistors (driver / audio) | — | Drift high → low output or weak audio | Measure and replace. |

Method: lift one lead, measure, and replace parts more than 10–20% out of tolerance. Use carbon comp or carbon film in RF and oscillator circuits.

## 3.3 Tubes, by risk

1. **6HS6 (V10, V11):** the top HW-101 receive killer. Try a 6AU6 as a test substitute.
2. **6146 finals (V8, V9):** emission, gas, mismatched pairs. Only service in the set proves a final.
3. **6EA8 (V1, V12, V15):** oscillation, relay hang-up, sidetone faults.
4. **6AU6 (V3, V4, V20):** brand-sensitive; S-meter zero and chirp problems.
5. **12AT7 (V19), 12AU7 (V16), 6BN8 (V13), 6GW8 (V14), 0A2 (V18).**

Put each tube back in the socket it came from. Alignment is tube-dependent.

## 3.4 Mechanical, switches, relays

| Item | Symptom | Fix (source) |
|---|---|---|
| **RL2 contacts (carbon)** | Chirp, slow receive recovery, no audio after TX. +300 V can leak onto the bias/AGC lines. | Clean contacts 2 and 10 with typing paper; clean carbon from the relay base (HW-101-32). Covers pull straight up. |
| RL1 / RL2 general | Chatter, sticking | Card-strip cleaning; VOX chatter bulletins -31, -41, -57. |
| **Bandswitch wafers** | Dead or partial bands | DeoxIT and rock-test. **Check wafer notch orientation.** One list unit had 3 of 5 wafers reversed from the original build. |
| L904 → bandswitch wires (5) | Short if run without a load | Sleeve or replace (K4JPN). |
| **SSB/CW filter slide switch** (by RF gain) | Low TX output **and** low RX sensitivity | Clean (HW-101-53). |
| Rotary switch detents | Broken detent | Newer detent 266-1116 (HW-101-66, -71). |
| **Driver preselector / load rubber belts** | Preselector won't track, slips | Silicone belts, 50 mm ID / 56 mm OD / 3 mm. Same belt for SB-100/101/102 and HW-100/101. |
| Preselector tuning drums | Loose on the shaft | Grind off the old solder, epoxy-bond (Latta). |
| Preselector caps (26-122) | Stiff; corroded pins | Clean or tin the pins; replace if they bind (HW-101-29, -81). |
| Loading cap turns with plate | — | Grommet 73-3 or fiber washer on the shaft (HW-101-30, -64). |
| **VFO vernier / stop stud** | Erratic VFO | Braid from the stop stud to a frame lug (HW-101-27). |
| VFO tune slips | — | Nylon and spring washers behind the loading pulley (HW-101-78). |
| VFO spiral and follower | Stiff dial | Clean the groove, silicone-lube, oil the pawls; regrease the capacitor if stiff (W5RKL). |
| Coil covers | Can't reach driver/het coils | Enlarge holes with a Dremel #194 bit (Latta). |
| RF driver board screws / lockwashers | Self-oscillation | Tighten and fit lockwashers (HW-101-9). |
| Carrier null pot (10-147) | Fails, noisy | Fiber washer and ground wire (HW-101-3); or a 200 Ω trimpot (KM5Z). |
| Pilot lamps | Low output, S-meter drift | **#47**, never #44; never run with a lamp out (HW-101-20). |

## 3.5 Calibrator and crystals

- **Y201 100 kHz crystal:** a choppy output causes spurs and raised noise, and can trip anti-VOX so the rig sits in transmit (HW-101-25; Latta). Replace (404-43).
- **Band crystals** are band frequency + 8.895 MHz; for example 29.5 MHz uses 38.395 MHz. They're shared with the SB-102. Latta's 29.5 MHz crystal was dead.
- **Carrier crystals** have wide tolerance. WB8JKR measured LSB 3393.8, USB 3395.9 and CW 3395.17 kHz, and pulled them back to 1.4–1.6 kHz either side of the filter center. CW output went from 50 W to 110 W.

## 3.6 HP-23 power supply (do this first)

| Item | Original | Failure | Replace with |
|---|---|---|---|
| C1, C2 (HV, in series) | 125 µF 450 V | Low capacitance (Latta) | 150 µF 500 V |
| C3, C4 (B+) | 125 µF | Aging | 150 µF 500 V |
| **C5** (B+ filter) | 40 µF 450 V | **Open → tone on CW, relay chatter** (HW-101-73) | 47 µF 450 V |
| C6, C7 (bias) | 40 µF 150 V | **Both open** (Latta) | 47 µF 160 V+ |
| R1–R5 (HV bleeders) | 100 kΩ 2 W | Measured ~150 kΩ | 100 kΩ 2 W metal film |
| R7 / R8 (bias) | 1 kΩ / 27 kΩ | Drift; R8 was 1 W | 2 W metal film |
| D1–D7 | 1N2071 | Usually OK | 1N4007 |
| Line cord | 2-wire | Safety | 18/3 grounded |

**Outputs (11-pin):**

| Pin | Output |
|---|---|
| 1 | −130 V bias |
| 2, 6 | 12.6 VAC filaments |
| 3 | B+ 250/300 V |
| 4 | HV ≈ 700 V loaded (~800 V unloaded) |
| 7 | Ground |
| 9–10 | Power-switch loop |

**The B+ switch must be on 300 V for the HW-101.** On 250 V you get low TX power, VOX trouble and V18 flicker on CW. The older 250/OFF/300 switch is easy to bump.

Restored-board kits (for example HP-23RL) replace most of these parts in one step.

# 4. Restoration plan

## 4.1 Phase 0: Assess cold

- Photograph everything. Note mods such as RIT added to V20, R202 present or jumpered, BNC/SO-239 output and non-stock tubes.
- Pull and label every tube by socket. Test them, and note the brands (El-Menco, GE 6EA8, IEC).
- **Check bandswitch wafer orientation** against the manual's pictorials.
- Inspect: R940/R701/R904 (scorching), the final cage, the RF choke (fragile wire), L904 wires, RL1/RL2 bases, belts and drums.
- Resistance-check the HW-101 B+, HV and bias lines to chassis, using the manual's chart, before connecting a supply.

## 4.2 Phase 1: HP-23 first

Recap the HP-23 as in §3.6. Bring it up on a variac at 10 → 25 → 50 → 75 → 100%, unloaded, with a dummy load resistor on each output. Confirm about −130 V bias, 300 V B+ (switch!) and about 800 V HV unloaded. Check its primary (plug, fuse, switch) with an ohmmeter first.

## 4.3 Phase 2: Clean

- Vacuum and brush; gentle compressed air.
- Use cleaner on a rag or swab, never sprayed directly. Test naphtha on a hidden spot first.
- Final cage: finals out; pipe-cleaner between the tuning-cap plates.
- Front panel and windows: mild cleaner only; plastic dulls easily.
- DeoxIT on every rotary switch (including the bandswitch), pot and the SSB/CW slide switch.

## 4.4 Phase 3: Transceiver recap and resistor survey

1. Replace C2, C12, C212, C304 and C906 (C906 negative polarity).
2. Replace the drift-prone resistors in §3.2: R106 (2 W), R107 (1 W), R6/R7 (1 W), R1 (1 W). Check R117, R915, R212, R940, R701 and R904.
3. Lift-and-measure survey of the remaining resistors, worst-heat areas first (audio, VFO buffer, driver, modulator).
4. Clean RL2 contacts and base, and RL1.
5. Fit #47 pilot lamps.

## 4.5 Phase 4: Mechanical

Fit silicone preselector/load belts, epoxy loose drums, ground the VFO stop stud, glue C946, and enlarge coil-cover holes if needed.

## 4.6 Phase 5: VFO

The VFO is solid-state with a tube buffer; it is not an LMO.

- Drift: try several V20 6AU6s (use the least-drifting). Check V20 pins 5/6 at about 149 V with VFO B+ at 150 V (V18 0A2). Suspect Q942 2N3393, which regulates the FET's ~8 V.
- Original-coil drift: coil 40-810 was superseded by 40-1976 (HW-101-1).
- Stops at high end: R947 1 kΩ → 470 Ω and a diode from Q941 gate to ground (HW-101-52), or lower T941 output to ~2 V RF (HW-101-77).
- Tracking: use the **lower** of the two coil-slug peaks (HW-101-45).
- Remove any RIT mod wired to the buffer tube (field report: it caused one-directional shift).

## 4.7 Phase 6: First power-up

1. HP-23 on a variac. **Dummy load (50 Ω, not a light bulb) on the antenna jack.** Speaker connected.
2. Raise slowly; watch for heat or smell. Both pilot lamps must light; they balance the filament string.
3. Log voltages against the manual's voltage chart.
4. Check receive on every band with the calibrator.
5. **Set final bias before any transmit** (§4.8 T1).

## 4.8 Phase 7: Alignment

Order matters: **receiver first**, because the transmitter shares receiver circuits. Warm up for 30 minutes. Use the plastic tool. **Never adjust the sealed 6.8 MHz trap.** T202 (bandpass) is factory set; touch it only with a sweep setup (HW-101-56).

**Receiver**

1. **S-meter zero:** 80 m, RF gain fully CW, METER on ALC. Adjust ZERO ADJ (IF board, behind the FREQ CONTROL switch).
2. **Heterodyne oscillator coils:** use an 11 MΩ meter (an analog meter must be ≥ 20 kΩ/V) at test point TP near V19 on the bandpass board.
   - Target: as high as possible between −0.5 and −2.0 V, **never above −2.0 V**.
   - Approach the setting from the slow-changing side of the peak.
   - Five coils are adjusted from the bottom (21, 7, 29.5, 29.0, 28) and three from the top (3.5, 14, 28.5). If TP reads 0 V, measure at the other end of the 100 kΩ resistor.
3. **IF:** calibrator at dial 400. Peak T201, T102 (top and bottom slugs), then T103. Repeat. WB8JKR peaks T102 for maximum transmit drive if there are two peaks.
4. **VFO:**
   - Set the dial with the Zero-Set button and slip clutch.
   - Dial 500 = 5000 kHz (VFO coil); dial 0 = 5500 kHz (side trimmers). Iterate.
   - The dial reads inverted: 5.000 MHz at 500.
   - Without a counter, verify against a known signal; the error can reach 100 kHz.
5. **Driver grid and plate coils:** in the order 3.5 (3.7) → 29 (29.2) → 21 (21.2) → 14 (14.2) → 7 (7.2). Set the driver preselector to match each band and peak the S-meter. On 40 m, the preselector may need to sit near 1 o'clock (HW-101-47).

**Receiver benchmark (W5RKL):** calibrator about **S9+20 at 3700 kHz**, falling to about **S6 at 29.2 MHz**.

**Transmitter** (50 Ω dummy load; short key-downs; let the finals cool ≥ 30–60 s between tries)

1. **Final bias:**
   - Set up: PTT, LSB, no drive (MIC/CW level at zero), METER on PLATE.
   - Adjust BIAS for the manual's resting plate current: the **50 mA mark/dot** (W5RKL); Latta reads it as "3" on his scale.
   - **Confirm the procedure in the manual.** Wrong bias for more than a few seconds can ruin the finals.
2. **Carrier isolation amp T1:**
   - Use the REL PWR meter in TUNE, keeping output ≤ 6.
   - Peak the preselector and FINAL.
   - Reduce MIC/CW level to ≤ 3 and peak T1. Repeat once.
3. **Preliminary neutralization (3.5 MHz):** peak RF output and plate-current dip must fall at the **same FINAL setting**. Adjust the cage-front neutralizing capacitor 1/16–1/8 turn at a time. If the two points move apart, go the other way.
4. **Carrier null (3700 kHz):**
   - Set up: LSB, PTT, MIC/CW fully CCW.
   - Null with the CARRIER NULL control and capacitor, then repeat on USB.
   - Aim for equal residual carrier on both sidebands, about ¼ V on the REL PWR meter or less. Spec is −45 dB.
   - If C14 bottoms out, see HW-101-28. If suppression is poor, change R9 from 1 kΩ to 390 Ω (HW-101-38).
5. **Final heterodyne-coil touch-up:** TUNE, REL PWR 3–9. Peak the 3.5 coil, then each band's coil.
6. **Driver neutralization (21.2 MHz):** set the white wire to the 6CL6 plate (hole W) so the preselector peaks smoothly. Shorten it to about 1/8 in. if final neutralization is hard (K4JPN). On 15 m, bend it flat if the driver oscillates (HW-101-79).
7. **Final neutralization touch-up at 14.2 MHz.** It is more sensitive there.
8. **Calibrator:** zero-beat a harmonic against WWV at 10 or 15 MHz with the CAL CRYSTAL trimmer, during an unmodulated period.
9. **VFO shift:** dial 200 on 3.5 MHz, CAL on. Zero-beat in USB, switch to LSB without touching the dial, and adjust SHIFT ADJUST on the VFO. Iterate.
10. **Loading:** dip with FINAL, load to about **40** on the HW-101 plate scale (≈ 225–250 mA). Re-neutralize if maximum REL PWR and the dip don't coincide.

Changing finals, tube brands or layout means re-neutralizing.

## 4.9 Phase 8: Optional improvements (reversible)

| Mod | Change (source) |
|---|---|
| Stable meter zero on TX | 10 MΩ from meter-zero pot pin 3 to T102 pin 2 (WB8JKR) |
| Carrier crystals on frequency | Series/parallel trim caps; LSB 3393.5, USB 3396.3, CW 3395.4 kHz (WB8JKR) |
| More BFO injection | C17 12 → 100 pF; R6/R7 → 27 kΩ (WB8JKR) |
| Less raspy audio | C119 500 pF → .001 µF; C306 → .01 µF (WB8JKR) |
| Better mic low end | C11 .001 → .01 µF (WB8JKR) |
| Faster CW relay pull-in | R328 470 kΩ → 1 kΩ (WB8JKR) |
| Sidetone bleed in receive | .001 µF RL1 pin 1 to ground; R326 → 2.2 MΩ (WB8JKR) or 3.3 MΩ (HW-101-16) |
| Sidetone volume control | HW-101-70 |
| Birdies | .01 µF from the common filament point to ground (bandpass board) |
| Low-Z headphones | WB8JKR rewiring; .01 µF across phone jack pins 2–3 (K4JPN) |
| RIT | QST Mar 1969 (SB-101 design) |
| Avoid | Replacing the RCA antenna jack with SO-239. Use an adapter (Latta). |

# 5. Bulletin quick-reference (symptom → bulletin)

| Symptom | Bulletins |
|---|---|
| S-meter drift / won't zero | -10, -20, -34, -67; R106 |
| AGC poor, fast S-meter decay | -24, -55 |
| VFO drift, warble, erratic, stops high | -1, -2, -19, -27, -42, -45, -52, -77, -80 |
| Relays hang / chatter / chirp | -23, -31, -32, -41, -57 |
| Low TX output or RX sensitivity | -11, -43, -46, -53, -56 |
| Carrier suppression / null | -3, -28, -38 |
| Finals / neutralizing / RF choke heat | -5, -18, -39, -49, -59, -61 |
| Calibrator spurs | -25 |
| R940 / R701 overheating | -37, -50, -75 |
| Sidetone | -16, -65, -70, -74 |

# 6. Operation notes

- Make every transmit adjustment into a **50 Ω dummy load**.
- Keep the HP-23 switch on **300 V**.
- Never transmit without a load; the L904 wiring and finals suffer.
- Tune up at reduced drive and keep key-down periods short.
- Use the calibrator and zero-set before netting.

# 7. Tips and tricks

- Label tubes by socket. Tube brand matters in this rig (6AU6, 6HS6, 6EA8).
- Photograph wafer and rotor orientation before touching the bandswitch.
- Grid-dip the heterodyne coils to find dead band crystals quickly (crystal = band + 8.895 MHz).
- Spare tubes worth keeping: 6HS6 ×2, 6EA8, 6AU6 (non-El-Menco), 12AT7, a matched 6146 pair.
- Keep #47 lamps, silicone belts and a 100 kHz crystal on hand.

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | 30 min warm-up; calibrator zero-set. |
| 6 months | Exercise switches and pots; check S-meter zero. |
| Yearly | Bias check; carrier null check; receive benchmark; clean relay contacts if chirp or slow recovery appears. |
| 2–3 years | Neutralization check (peak and dip coincide); retighten board and driver screws; DeoxIT switches. |
| 5 years | Test all tubes (6HS6 and finals first); inspect belts; HP-23 voltage check and capacitor ESR/leakage. |
| ~15–20 years | Re-check electrolytics in both units. |

# 9. Parts sources

- Mouser / Digi-Key: electrolytics, 2 W metal-film and wirewound resistors, NPO caps, 1N4007.
- HP-23 recap and rebuild boards (HP-23RL and similar).
- Silicone preselector belts (50/56/3 mm) from Heathkit vendors and eBay.
- Heathkit-Radios (groups.io): crystals, 6HS6s, donor boards.

# 10. Sources

- Heath, *HW-101 Assembly & Operation Manual*; *HW-101 Service Bulletins* 1971–79 (Latta site, frostburg.edu).
- Heath list (puck.nether.net), Jul 1997: transcribed bulletins HW-101-1 to -55.
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*.
- Latta, Frostburg State: HW-101 restoration, alignment, modifications, HP-23 restoration and schematic pages.
- WB8JKR, *Optimizing the HW-101, SB100-102 Transceivers*; K4JPN, *Improve Your HW-101*; KM5Z refurb summary (pestingers.net).
- Heath list threads: "Problems with HW-101 — a possible solution" (Dec 2020, reversed wafers); "Drifting HW-101" (Jun 2007); "S meter drift" (Feb 2003).

# 11. Pending manual cross-check

As with the SB-301, I'll check this plan against the original manual. Items to verify:

- V-number assignments (§2.2).
- The full electrolytic list and voltage ratings (C2/C12/C212/C304/C906 and any others).
- The exact bias procedure and resting-current target.
- The resistance and voltage charts.
- Diode designators and types.
- The manual's alignment wording and troubleshooting chart.
