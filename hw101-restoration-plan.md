---
title: "Heathkit HW-101 Transceiver — Restoration Plan"
subtitle: "Rev. 2 — checked against the original Heath manual (595-1277-18)"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
toc: true
---

<!-- Build: pandoc hw101-restoration-plan.md -o hw101-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **Safety.** The HW-101 runs on **+700–850 V DC** from the HP-23 power supply (manual spec, p. 159). That voltage stays on the filter capacitors after the supply is unplugged. The manual (p. 117) says to connect **a lead from a good earth ground to the chassis GND terminal and to all test equipment** before power-up, and warns that lethal voltages are present above and below the chassis. Discharge before every hands-in session, and never operate the final cage with its cover off. Work one-handed when the set is live.

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath HW-101 Manual** 595-1277-18 (©1970; late printing, 245 pp. scan) | Supplied by owner; also on Latta's site | **Primary reference.** Page numbers below are the manual's printed numbers. |
| **HW-101 Service Bulletins** HW-101-1 … -85 (1971–79) | Latta site; -1 to -55 transcribed on the Heath list (Jul 1997) | Factory fixes. Several are already built into this late printing (§11). |
| HP-23 power supply manual | Latta site | Companion supply. |
| W5RKL, *HW-101 and SB-10x Restoration and Troubleshooting* | w5rkl.com | First power-up and checks. |
| Latta (Frostburg) HW-101 restoration, alignment, HP-23 pages | frostburg.edu | A documented restoration with measured failures. |
| WB8JKR, *Optimizing the HW-101 / SB-100–102*; K4JPN, KM5Z notes | mikeyancey.com, nostalgickitscentral.com, pestingers.net | Improvements and tube-brand issues. |
| Heath list / Heathkit-Radios (groups.io) | puck.nether.net, groups.io | Field experience. |

**Numbering (schematic notes):** 1–99 modulator board; 100s IF; 200s bandpass; 300s audio; 400s RF driver; 500s crystal; 600s heterodyne oscillator; 700s driver grid; 800s driver plate; 900s chassis. Resistors are 1/2 W unless marked. Manual voltages were measured with an 11 MΩ VTVM: ±10% (p. 144), ±20% on the transmitter chart.

# 2. Transceiver at a glance

## 2.1 Factory specifications (pp. 157–160)

| Item | Spec |
|---|---|
| Coverage | 3.5–4.0, **7.0–7.3**, 14.0–14.5, 21.0–21.5, 28.0–30.0 MHz (four 10 m segments) |
| Sensitivity | **< 0.3 µV** for 10 dB (S+N)/N, SSB |
| Selectivity | SSB 2.1 kHz @ −6 dB, 7 kHz @ −60 dB (3.395 MHz filter). Optional SBA-301-2 CW: 400 Hz / 2.0 kHz. |
| Receiver audio | 2 W at < 10% distortion |
| Transmitter input | 180 W PEP SSB; 170 W CW (50% duty) |
| Output | **100 W on 80–15 m; 80 W on 10 m** into 50 Ω |
| Suppression | Carrier and unwanted sideband −45 dB; 3rd-order IMD −30 dB; harmonics −40 dB |
| Stability | < 100 Hz/hr **after 45 min warm-up**; < 100 Hz for ±10% line |
| Dial | 5 kHz divisions; 35⅓ knob turns per 500 kHz |
| Complement | **20 tubes, 19 semiconductors** (p. 3); MPF-105 FET VFO, 2N3393 regulator |
| **Power requirements** | **700–850 V @ 250 mA (1% ripple); 300 V @ 150 mA; −115 V @ 10 mA; 12 V AC/DC @ 4.76 A** |
| Pilot lamps | **#47** (412-1), ×2 (parts list p. 45) |
| Weight / size | 17½ lb; 14-13/16 × 6-5/16 × 13-3/8 in. |

## 2.2 Tube complement (spec p. 159; V numbers from schematic, block diagram and troubleshooting chart)

| V | Tube | Function | Known issues (source) |
|----|----------|--------------------|----------------------------------------------|
| V1 | 6EA8 | Speech amp (A) / cathode follower (B) | GE 6EA8s oscillate (~65 kHz) → VOX chatter, no carrier null (HW-101-21, -31). Manual: "very low output in USB/LSB → V1 defective" (p. 152 #33). |
| V2 | 6AU6 | Isolation amplifier | — |
| V3 | 6AU6 | 1st IF amp (TX and RX) | Leakage → S-meter drift (HW-101-34). El-Menco draws excess current → can't zero. |
| V4 | 6AU6 | 2nd IF amp | Brand-sensitive (HW-101-46). |
| V5 | 6EA8 | 1st transmit mixer (V5A) | Needs VFO injection (p. 152 #34). |
| V6 | 6CB6 | 2nd transmit mixer | Under shield. |
| V7 | 6CL6 | Driver | **Weak V7 → transmit output falls off** (p. 153 #42). |
| V8, V9 | **6146** (411-75) | Finals | Gassy 6146s → output falls off (p. 153 #42). Heath: 6146A only (HW-101-18, -39), later GE 6146B approved (HW-101-49). **Re-neutralize after any change.** |
| V10 | 6HS6 | RF amplifier | **High failure.** Leakage → poor AGC (HW-101-24). A 6AU6 substitutes directly. |
| V11 | 6HS6 | 1st receive mixer | Same as V10; some 6HS6s had swapped pinouts (HW-101-12). |
| V12 | 6EA8 | 2nd receive mixer (A) / relay amp (B) | Defective V12 → relays stay energized (p. 153 #39; HW-101-23); relays won't pull in TUNE (V12B, p. 152 #37). |
| V13 | 6BN8 | Product detector (C) / AVC | Latta found a weak one. |
| V14 | 6GW8 | Audio amp (A) / audio power (B) | Runs hot. |
| V15 | 6EA8 | CW tone oscillator / amplifier | Shorts → sidetone faults, continuous keying (HW-101-74). |
| V16 | 12AU7 | Carrier oscillator / BFO | Plate resistors R6/R7 drift (§3.2). |
| V17 | 12AT7 | VOX amp (A) / 100 kHz calibrator (B) | — |
| V18 | 0A2 | 150 V regulator | Fed through 7 W resistors R304/R305 (§3.2). |
| V19 | 12AT7 | Heterodyne osc (A) / cathode follower (B) | Repeated failure: R212 220 → 330 Ω (HW-101-2). |
| V20 | 6AU6 | VFO amplifier | **"Output low" → V20 weak** (VFO chart, p. 155). Its cathode feeds the FET's regulated supply → drift. |

**Brand note (K4JPN, W5RKL):** El-Menco 6AU6, 6HS6, 6EA8, 12AT7, 12AU7 and 6CL6 tubes are repeatedly implicated. Keep spares of other brands.

## 2.3 Semiconductors (schematic diode/transistor tables; parts lists)

| Designators | Type | Heath p/n | Use |
|---|---|---|---|
| D1, D2, D101, D201, D301, D902–D906 | 1N2071 (1 A, 600 PRV) | 57-27 | Switching, ALC rectifiers, anti-trip rectifiers, DC blocking |
| D901 | 1N4149 | 56-56 | — |
| **CR1–CR4** | **FH-1100 hot-carrier** in this printing (1970 schematic table: 1N191) | 56-87 | Balanced modulator (HW-101-13 upgrade) |
| CR201, CR901, CR941 | 1N191 germanium | 56-26 | Calibrator harmonic generator, RF sampling, VFO shifter |
| D202 | 15 V zener (1N4166A) | 56-25 | Cathode bias |
| Q941 | MPF-105 FET | 417-169 | VFO oscillator |
| Q942 | 2N3393 | 417-118 | Voltage regulator (VFO supply) |

Diode faults named in the manual:

- **D101 → slow receive recovery** (p. 153 #41).
- **D201 wrong type or backwards, D202 backwards or defective → no VOX** (p. 152 #38).
- **CR941 reversed → VFO shifter dead** (p. 155).
- **Q941/Q942 wired wrong → no VFO output** (p. 155).
- Bulletin HW-101-11's "D907" does not appear in this manual's diode table; check which diode it refers to on your unit.

# 3. What fails, ranked

## 3.1 Capacitors

**All five electrolytics** (parts lists pp. 12, 20, 24, 44; schematic):

| Ref | Value (p/n) | Board / role | Replace with |
|---|---|---|---|
| C2 | 10 µF (25-115) | Modulator; V1A speech amp cathode bypass | 10 µF 50 V |
| C12 | 20 µF (25-135) | Modulator; speech amp B+ decoupling (+300 V line via R14 4.7 kΩ) | 22 µF 450 V |
| C212 | 10 µF (25-115) | Bandpass; VOX amp cathode bypass | 10 µF 50 V |
| C304 | 20 µF (25-135) | Audio; audio-stage B+ decoupling | 22 µF 450 V |
| C906 | 10 µF (25-147) | Chassis; **bias filter on the −130 V line (positive lead to ground)** | 10 µF 250 V, **polarity reversed vs. B+ caps** |

The manual does not print voltage ratings. The ratings above come from the circuit voltages (+300 V B+, −130 V bias). Keep audio and speech capacitances the same.

| Other capacitor | Likelihood | Symptom | Action |
|---|---|---|---|
| **C213** (VOX delay) | Medium | **Relays stay energized** (p. 153 #39C); VOX delay drift | Leakage-test; .47 µF per HW-101-63. |
| C946 (4700 pF, VFO) | Medium | Warble when tapped | Glue to the VFO wall (HW-101-42). Manual: separate the two **56 pF discs** by > 1/8 in. to cure VFO microphonics (p. 155). |
| C947 (VFO shift) | Medium | Intermittent shift | 56 → 47 pF NPO (HW-101-19). |
| C208, C223, C604 | Low–medium | No heterodyne injection (p. 152 #36) | Check value. |
| .005 µF 1.6 kV discs (×2, chassis) | Medium | HV bypass short | Replace with 2 kV+ parts. |
| .2 µF Mylars (27-34; modulator ×3, IF, bandpass, chassis ×2) | Medium | Leakage | Replace with film. |
| C701, C801 (680 pF) | Medium | R940 overheats | 680 pF 500 V (HW-101-75). |
| Ceramic discs, micas | Low | — | Spot-test. |

## 3.2 Resistors

These are 1/2–7 W carbon composition and wirewound parts, ~50 years old. Carbon comps drift high, worst where they run hot.

| Ref | Manual value | Failure / symptom | Fix (source) |
|---|---|---|---|
| **R106** | **22 kΩ 1 W** (1-5-1, IF board; 1 W already in this printing) | Heats and changes → S-meter zero drift | 22 kΩ 2 W wirewound (WB8JKR) or 33 kΩ 2 W (K4JPN). Keep all four connections on its pad. |
| **R107** | **100 kΩ 1 W** (1-28-1; HW-101-10 already incorporated) | S-meter drift | Check that it is the 1 W part. |
| **R6, R7** | **33 kΩ 1/2 W** (6-333; V16 plates) | Latta measured **330 kΩ** and 42 kΩ → low USB/CW gain | 33 kΩ 1 W (HW-101-84); WB8JKR uses 27 kΩ. |
| **R1** | 100 kΩ (V1A plate) | Heat damage | 1 W (HW-101-84); also check R215 and R316. |
| **R302** | **3.3 kΩ 1 W** (bias series, −130 V → bias line) | Drift → wrong bias range | Measure; replace with 2 W metal film. |
| **R304, R305** | **1 kΩ 7 W, 2.5 kΩ 7 W** (0A2 / 150 V dropping) | Hot wirewounds; open → no 150 V (VFO, heterodyne oscillator) | Measure; check the surrounding board for heat damage. |
| R117 | 3.3 kΩ (AVC) | Open → AVC and S-meter decay too fast | Replace (HW-101-55). |
| R915 | 3.3 MΩ | Open → no SSB output, TUNE OK | Replace (HW-101-69). |
| R940, R904, R701 | 100 Ω / 1 kΩ | Overheat or short to the driver shield | Sleeve and dress (HW-101-37, -50; W5RKL). |
| R212 | 220 Ω | Repeated heterodyne-oscillator tube failure | 330 Ω (HW-101-2). |
| **R306, R307** | (VFO shift network) | **VFO doesn't shift with the Mode switch** (p. 152 #35) | Check value. |
| R221 | — | Not soldered at bandpass point F → **VFO output high** (p. 155) | Resolder. |
| R202 | 10 kΩ | *Early units only.* Limits drive | Jumper it (HW-101-14). |
| 2.2 Ω 2 W (chassis) | — | — | Check. |

Method: lift one lead, measure, and replace parts more than 10–20% out. Use carbon comp or carbon film in RF and oscillator circuits.

## 3.3 Tubes, by risk

1. **6HS6 (V10, V11):** the top HW-101 receive killer. Try a 6AU6 as a test substitute.
2. **6146 pair (V8, V9):** gas or emission ("output falls off", p. 153).
3. **6CL6 driver (V7):** weak → falling output.
4. **6EA8 (V1, V5, V12, V15):** oscillation, relay hang, sidetone.
5. **6AU6 (V3, V4, V20):** brand-sensitive; V20 weak → low VFO output.
6. **12AT7 (V17, V19), 12AU7 (V16), 6BN8, 6GW8, 0A2.**

Put each tube back in the socket it came from.

## 3.4 Mechanical, switches, relays

| Item | Symptom | Fix (source) |
|---|---|---|
| **RL2 contacts (carbon)** | Chirp, slow recovery | Clean contacts 2 and 10; clean the relay base (HW-101-32). |
| RL1/RL2 | Won't energize | V12B, relay or Mode wafer 2F (p. 152 #37). |
| **Band/crystal switch wafers** | Dead bands; no heterodyne injection | Manual: **"rotors reversed 180°" on the crystal or heterodyne-oscillator switch wafers** (p. 152 #36). A field unit had 3 of 5 wafers reversed. |
| **Modulator and RF driver board hardware** | **Transmitter unstable** | Tighten; **fit the lockwashers between the boards and chassis** (p. 153 #40B; HW-101-9). Also coil shield cover, tube-shield clips, and V8/V9 socket hardware (p. 153 #40). |
| SSB/CW filter slide switch | Low TX and RX | Clean (HW-101-53). |
| Driver preselector/load belts | Slip, won't track | Silicone 50/56/3 mm. |
| Preselector drums | Loose | Epoxy (Latta). |
| VFO / Jackson drive | Erratic, slips | Braid from the stop stud to the frame (HW-101-27); washers (HW-101-78). The VFO and dial come out as a unit (p. 154). |
| Comb brackets / detents | — | This printing already ships **steel comb brackets 204-2096** and **detents 266-1116**. Earlier units may need retrofitting (HW-101-8, -66). |
| Pilot lamps | Low output, S-meter drift | **#47 only** (manual p. 45; HW-101-20). |

## 3.5 Calibrator and crystals

- **Supplied heterodyne crystals** (Y501–Y508) cover only the specified segments. Heath warns against other heterodyne crystals, because the driver coils are sequence-tuned in series-parallel (p. 120). A dead crystal gives no injection on that band (p. 152 #36B).
- **"Birdies" near 3740 kHz and 21,200 kHz are normal** (p. 120).
- Carrier crystals (modulator board): **3393.6, 3396.4, 3395.4 kHz** (404-205/-206/-215; p. 12). WB8JKR retrimmed drifted crystals: CW output went from 50 W to 110 W.
- Y201 100 kHz calibrator crystal: a choppy output causes spurs and noise, and can latch the rig in transmit through anti-VOX (HW-101-25; Latta).

## 3.6 HP-23 power supply (do this first)

**Manual (p. 114):**

- Set the **HP-23A's low-voltage switch to 300 V DC**. On the HP-23B, HP-23C and PS-23, make the internal connections for 300 V.
- **With the HW-101, the HP-23's own BIAS control is inoperative**; bias is set by the transceiver's BIAS control.
- With an **HP-13**, set the supply's bias control **fully clockwise** (p. 113).
- **Troubleshooting chart:** "relays stay energized → low B+, at 250 V instead of 300 V" (p. 153 #39E).

**11-pin plug (Fig. 1-6):**

| Pin | Function |
|---|---|
| 1 | −130 V bias |
| 2 | Filament common |
| 3 | LV +300 V |
| 4 | HV +800 V |
| 5 | Ground (relay) |
| 6 | 12.6 V filament |
| 7 | Ground (B− and bias) |
| 8 | No connection |
| 9–10 | Power switch |
| 11 | Relay N.O. (linear amplifier) |

**Typical recap (Latta, field):**

| Item | Original | Failure | Replace with |
|---|---|---|---|
| C1–C4 | 125 µF 450 V | Low capacitance | 150 µF 500 V |
| C5 (B+) | 40 µF | Open → CW tone, relay chatter (HW-101-73) | 47 µF 450 V |
| C6, C7 (bias) | 40 µF 150 V | Open | 47 µF 160 V+ |
| R1–R5 bleeders | 100 kΩ 2 W | ~150 kΩ | 100 kΩ 2 W metal film |
| D1–D7 | 1N2071 | Usually OK | 1N4007 |
| Line cord | 2-wire | Safety | 18/3 grounded |

# 4. Restoration plan

## 4.1 Phase 0: Assess cold

- Photograph everything; note mods (RIT on V20, R202 present or jumpered, output connector changes, non-stock tubes).
- Pull and label every tube; test them; note brands.
- **Check switch-wafer rotor orientation**, especially the crystal and heterodyne-oscillator wafers.
- Inspect R304/R305 (7 W), R940/R701/R904, the final cage and RF choke, RL1/RL2, belts and drums.
- Check the receiver against the **resistance chart, Fig. 1-22** (fold-out from p. 155), with the power plug disconnected and all controls fully CCW. Sample values from the chart:

| Point | Expected |
|---|---|
| V8/V9 plate | ~95 kΩ |
| V18 | 34 kΩ |
| V19 | 38 kΩ / 9 kΩ |
| V17 pin | 0 Ω with FUNCTION in CAL |

## 4.2 Phase 1: HP-23 first

Recap per §3.6. Variac bring-up, unloaded, with test loads. Confirm about −130 V bias, **300 V** B+ and about 800 V HV.

## 4.3 Phase 2: Clean

- Vacuum and brush; cleaner on a rag or swab, never sprayed.
- Final cage: pipe-cleaner between the tuning-cap plates.
- DeoxIT every rotary switch, pot and slide switch (Function, Meter, SSB/CW filter).

## 4.4 Phase 3: Recap and resistor survey

1. Replace C2, C12, C212, C304 and C906 (C906 reversed polarity).
2. Replace the .005 µF 1.6 kV HV discs and the leaky Mylars.
3. Resistors per §3.2: R106 (2 W), R6/R7 (1 W), R1 (1 W), R302; check R304/R305, R117, R915, R212, R306/R307.
4. Clean RL2/RL1.
5. Fit #47 lamps.

## 4.5 Phase 4: Mechanical

Fit silicone belts, epoxy the drums, braid the VFO stop stud, glue C946 and separate the 56 pF discs. Tighten the board hardware **with lockwashers**.

## 4.6 Phase 5: VFO (pp. 154–155)

The VFO is a field-effect transistor oscillator (Q941) with a V20 6AU6 amplifier and Q942 2N3393 regulator, fed from the 0A2's +150 V.

| Symptom (VFO chart) | Cause |
|---|---|
| Output low | V20 weak; or peak the top core of coil FM at dial 300 |
| Output high | R221 not soldered at point F; VFO coil core turned to the wrong end (should be ~1¼ in. below the coil end) |
| No output | Q942 backwards; Q941 wired wrong; C950 lugs touching chassis; output cable shorted; C951 lead not through lug 2 to TP of L941 |
| Shifter doesn't work | CR941 reversed |
| Microphonics | 56 pF discs too close; long leads |

**Removing the VFO (p. 154):**

1. Pull V19.
2. Remove the pilot-lamp screw.
3. Unplug the coax.
4. Unsolder bias, B+ and filament.
5. Loosen the Jackson-drive setscrews.
6. Remove the knob and the 3 and 9 o'clock screws.
7. Remove the spade-bolt nuts.
8. Work the VFO up and back.

When reconnecting: white-gray → BIAS, orange → B+, brown → FIL.

Field drift fixes: try several V20s; check Q942; coil 40-810 → 40-1976 (HW-101-1); remove RIT mods wired to the buffer.

## 4.7 Phase 6: Initial test (p. 117)

1. **Earth-ground the chassis and the test gear.**
2. Set the controls:

   | Control | Setting |
   |---|---|
   | FINAL TUNE | 10 o'clock |
   | LOAD | 4 o'clock |
   | MODE | LSB |
   | FUNCTION | PTT |
   | METER | PLATE |
   | RF GAIN | Full CW |
   | AF GAIN | PWR OFF |
   | Others | Fully CCW |
   | **BIAS (right side)** | **Fully CCW** |

3. Connect an **8 Ω speaker; never run without a speaker or phones.**
4. Turn on. The pilot lamps should light, the filaments glow, and the meter read zero. Turn AF GAIN up until noise is heard. If there's no noise, check that the transmitter isn't keyed (PTT, key or TUNE).
5. Measure:

| Point | Expected |
|---|---|
| Bandpass board point 5 | **+275 V** |
| Audio board point 4 | **−108 V** (bias) |

Then compare against the **Receiver DC Voltage Chart (Fig. 1-20)** and **Transmitter Voltage Chart (Fig. 1-21)**, fold-out from p. 144.

## 4.8 Phase 7: Alignment (pp. 119–125)

**Equipment:**

- 11 MΩ VTVM (a 20 kΩ/V VOM loads the circuits).
- **50 Ω nonreactive dummy load rated 100 W; never light bulbs.**
- A receiver for WWV or a broadcast station on a 100 kHz multiple.
- A scope or SB-610 is recommended.

**Do not transmit until directed** (p. 119).

**Presets (p. 119):**

- CAL XTAL trimmer notch toward the 100 kHz crystal.
- DRIVER PRESELECTOR at 12 o'clock.
- MIC/CW fully CCW.
- LSB, BAND 3.5, dial 200, PTT, RF GAIN full CW, METER on ALC, AF GAIN at 9 o'clock.

**Receiver**

1. **S-meter:** with the antenna off and RF GAIN full CW, set ZERO ADJ (right side) for zero.
2. **Heterodyne oscillator (p. 120):**
   - VTVM (−DC) to **TP on the bandpass board near V19**. If it reads 0, use the adjacent lead of the 100 kΩ resistor.
   - "−.82 V or higher is normal."
   - On every band, adjust that band's heterodyne coil for **about −0.5 to −2 V**. Adjust in the direction that gives the **slower** change.
   - Coils 3.5, 14 and 28.5 are adjusted from the top of the RF driver board; the rest from the bottom (positions are marked on the shield cover).
   - Latta adds: don't exceed −2.0 V.
3. **VFO trimmers:** turn both trimmer screws (through the VFO chassis) CW until just snug, then ¼ turn CCW.
4. **CAL check:** FUNCTION CAL, BAND 3.5, rock the dial around 400. The signal should stop on VOX and peak with the preselector. Reset the preselector to 12 o'clock.
5. **IF:**
   - Peak **T201** for maximum volume.
   - Peak **T102** top and bottom slugs.
   - Turn **T103** slug *up* (CCW) **14 turns**, then peak (no more than ~1 turn).
   - Retouch all three.
   - Don't turn T201/T103 cores more than two turns; **never touch the sealed 6.8 MHz trap.**
6. **VFO (p. 121):**
   - Warm up ≥ **30 min**.
   - Find CAL near 4000 kHz (500), turn the dial to its CCW stop, and slip the circular dial so the "Stop" end meets the hairline (Detail 8-9A).
   - Walk the 4000 kHz CAL signal onto **500** with the **VFO COIL**.
   - Set the 3500 kHz CAL signal onto **0** with **both VFO trimmers**.
   - Repeat until both coincide. (VFO is 5000 kHz at "500", 5500 kHz at "0".)
7. **Driver grid and plate coils (pp. 121–122)**, in this order, peaking the S-meter:

   | Band | Preselector | Coils |
   |---|---|---|
   | 3.5 | 12 o'clock | 3.5 |
   | 29.0 | 29.2 position | 29 |
   | 21.0 | 21.2 | 21 (the VFO harmonic sits near CAL; identify CAL with CAL/VOX) |
   | 14.0 | 14.2 | 14 |
   | 7.0 | — | 7 |

**Receiver benchmark (manual p. 122):** **at least S9+20 dB at 3700 kHz, decreasing to S3 at 29.2 MHz** on the calibrator.

**Transmitter** (pp. 122–124; **coil cover must be on**)

1. **Neutralizing capacitor preset** (new build): through the hole in the RF cage front, turn CW until resistance is felt, then **one full turn CCW**.
2. **Bias:**
   - Set up: preselector 12 o'clock, MIC/CW CCW, FINAL at 10 o'clock, LOAD at 4 o'clock, LSB, 3.5, dial 200, PTT, METER on PLATE.
   - **Press PTT and set BIAS so the needle sits on the ▼ mark (above the "3") = 50 mA resting cathode current.** Press PTT only a few seconds at a time until set.
   - Check that REL PWR reads 0.
3. **T1:**
   - TUNE, raise MIC/CW for an indication, peak the preselector and FINAL.
   - Set MIC/CW for **≤ S3**.
   - Turn T1's slug **up 14 turns**, then peak (≤ ~1 turn).
   - Reduce MIC/CW and re-peak T1.
4. **Preliminary neutralization (3.5 MHz):**
   - In TUNE, find the FINAL setting for minimum plate current and the one for maximum REL PWR.
   - Adjust the neutralizing capacitor a small amount at a time until they **coincide**.
   - Run full power only briefly; **cool ≥ 30 s** in LSB between tries.
5. **Carrier null:**
   - MIC/CW fully CCW. In LSB with PTT, null with the CARRIER NULL **control**, then the CARRIER NULL **capacitor**.
   - Repeat in USB, and repeat both until the residual is about equal on LSB and USB.
   - With an 11 MΩ VTVM and RF probe at the ANTENNA jack center pin, aim for **¼ V or less**.
6. **Heterodyne coils for drive:**
   - TUNE, REL PWR.
   - Peak the preselector, FINAL and LOAD; set MIC/CW for **3–9**.
   - Peak HET OSC coil 3.5 on the **slow side**, then each band's coil.
7. **Driver neutralization (21.2 MHz):** peak, then rock the preselector. If peaking is ragged, reposition or bend the **white insulated driver-neutralizing wire in hole W** of the RF driver board.
8. **Final neutralization (14.2 MHz):** peak, then adjust until maximum REL PWR and minimum plate current coincide.
9. **ALC zero:**
   - Re-zero the ALC position while receiving at 29.5, then check zero on every band.
   - If a band isn't zero, retouch that band's heterodyne coil.
   - The meter resting below zero on transmit is normal.
10. **Crystal calibrator (p. 124):**
    - Antenna wire to an external receiver tuned to WWV.
    - FUNCTION CAL, AF GAIN CCW. Adjust the **CAL XTAL trimmer** (bandpass board) for zero beat.
    - **Use 15 MHz WWV:** a 20 Hz error at 100 kHz becomes 740 Hz at 3.7 MHz.
11. **VFO shifter (p. 125):** 3.5, dial 200, CAL. Zero beat in **USB**, switch to **LSB** without moving the dial, and adjust **SHIFT ADJUST** on the VFO. Recheck USB.
12. **Dial calibration (p. 125):** zero beat the calibrator at 3.5 MHz / dial 0. Hold **ZERO SET** and turn the knob to bring 0 under the index. Verify on a known station (e.g. CHU 7335 kHz).

## 4.9 Phase 8: Operating tune-up (pp. 140–141): every session, and after any ±20 kHz move

1. Set the band and frequency. METER on PLATE, MIC/CW fully CCW, 50–75 Ω load.
2. **TUNE:** the meter should read **50 mA (▼)**. If it doesn't, redo the bias.
3. METER on REL PWR, LOAD at 4 o'clock, FINAL to the band's mark (Fig. 1-19).
4. Raise MIC/CW slightly. Peak the PRESELECTOR, FINAL and LOAD.
5. Raise MIC/CW until the output stops increasing, then re-peak.
6. METER on PLATE: about **"40" on the scale = 250 mA**. (Scale: 3 = 50, 6 = 100, 9 = 150, 20 = 200, 40 = 250, 60 = 300 mA.)
7. **Check:** maximum REL PWR must fall at the same FINAL setting as the plate dip. **If it doesn't, do not operate; re-neutralize.**
8. Return MIC/CW to CCW.

**Limits:**

- **Full output no longer than 30 s at a time; cool ≥ 1 min.**
- SSB (PTT, METER on ALC): set MIC/CW so voice peaks reach about **S3, keeping below S6** on the ALC scale.

## 4.10 Phase 9: Optional improvements (reversible)

| Mod | Change (source) |
|---|---|
| Stable meter zero on TX | 10 MΩ from meter-zero pot pin 3 to T102 pin 2 (WB8JKR) |
| Carrier crystals on frequency | Series/parallel trim caps (WB8JKR) |
| More BFO injection | C17 12 → 100 pF; R6/R7 → 27 kΩ (WB8JKR) |
| Less raspy audio | C119 500 pF → .001 µF; C306 → .01 µF (WB8JKR) |
| Mic low end | C11 .001 → .01 µF (WB8JKR) |
| Faster CW relays | R328 470 kΩ → 1 kΩ (WB8JKR) |
| Sidetone | R326 → 2.2 or 3.3 MΩ; volume control per HW-101-70 |
| Birdies | .01 µF from the common filament point to ground |
| Avoid | Replacing the RCA antenna jack with SO-239; use an adapter (Latta) |

# 5. Symptom → cause quick reference (manual chart pp. 146–155 + bulletins)

| Symptom | Manual chart / bulletins |
|---|---|
| Very low USB/LSB output | T1 not aligned; low mic level; V1 (p. 152 #33) |
| No VFO injection at V5/V12 | V20; power (p. 152 #34) |
| VFO won't shift with mode | Mode wafer 1F; R306/R307 (p. 152 #35); CR941 (p. 155) |
| No heterodyne injection at V6/V11 | Crystal Y501–Y508, coax, C208/C223/C604, V19, L601–L608, no 150 V, reversed wafer rotors (p. 152 #36) |
| Relays won't energize (TUNE / VOX) | V12B, RL1/RL2, wafer 2F; VOX SENS, V17A, D201, D202, anti-trip (p. 152 #37–38) |
| Relays stay energized | PTT/key closed, **C213**, **V12**, **B+ at 250 V not 300 V** (p. 153 #39); -23 |
| Transmitter unstable | Neutralization, **board lockwashers**, L801–L805, coil cover, tube-shield clips, V8/V9 hardware, lead lengths (p. 153 #40); -9 |
| Slow receive recovery | **D101** (p. 153 #41); RL2 carbon (-32) |
| Output falls off | Heat, bias, load, **gassy 6146s, weak V7** (p. 153 #42) |
| Dial zero varies band to band | Calibrator not exactly 100 kHz; heterodyne coils; VFO alignment (p. 153 #43) |
| Low output, all modes | IF-board wire not in hole 3 on the bandpass board (p. 153 #44) |
| S-meter drift / won't zero | R106, R107 (-10), V3 leakage (-34), #47 lamps (-20), -67 |
| Poor AGC | 6HS6 leakage (-24); R117 (-55) |
| Carrier suppression | -3, -28, -38 |

# 6. Operation notes

- 50–75 Ω nonreactive load always (manual p. 139). HP-23 on **300 V**.
- Earth-ground the chassis.
- Follow the 10-step tune-up every time; check that peak and dip coincide.
- Leave ventilation clear; don't stack equipment on the rig (p. 131).

# 7. Tips and tricks

- Label tubes by socket; tube brand matters (6AU6, 6HS6, 6EA8).
- Photograph wafer and rotor orientation before touching switches.
- Grid-dip heterodyne coils to find a dead crystal fast.
- Spares worth keeping: 6HS6 ×2, 6EA8, non-El-Menco 6AU6s, 6CL6, 12AT7, a matched 6146 pair, #47 lamps, silicone belts.

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | 45 min warm-up for the stability spec (30 min minimum); calibrator zero-set; 10-step tune-up. |
| 6 months | Exercise switches and pots; S-meter zero. |
| Yearly | Bias (50 mA ▼); carrier null; receive benchmark (≥ S9+20 at 3.7 MHz); relay cleaning if needed. |
| 2–3 years | Neutralization check at 3.5 and 14.2; retighten board hardware and lockwashers; DeoxIT. |
| 5 years | Tube test (6HS6, 6146, 6CL6 first); belts; HP-23 check. |
| ~15–20 years | Re-check electrolytics. |

# 9. Parts sources

- Mouser / Digi-Key: electrolytics, 2 W metal film, NPO caps, 1N4007, 2 kV discs.
- HP-23 rebuild kits (HP-23RL and similar); silicone belts; Heathkit-Radios (groups.io) for crystals, 6HS6s and boards.

# 10. Sources

- Heath, *Heathkit Manual for the SSB Transceiver Model HW-101*, 595-1277-18 (©1970).
- Heath service bulletins HW-101-1 … -85 (Latta site; Heath list, Jul 1997).
- W5RKL, *HW-101 and SB-10x Restoration and Troubleshooting*.
- Latta, Frostburg State HW-101 pages (restoration, alignment, modifications, HP-23).
- WB8JKR, *Optimizing the HW-101, SB100-102*; K4JPN, *Improve Your HW-101*; KM5Z refurb summary (pestingers.net).
- Heath list threads: "Problems with HW-101 — a possible solution" (Dec 2020); "Drifting HW-101" (Jun 2007); "S meter drift" (Feb 2003).

# 11. Rev. 2 change log (manual review)

| Item | Rev. 1 | Rev. 2 (per manual) |
|---|---|---|
| Power requirements | HP-23 outputs only | Spec: **700–850 V @ 250 mA, 300 V @ 150 mA, −115 V @ 10 mA, 12 V @ 4.76 A** (p. 159) |
| HP-23 bias | Not addressed | HP-23 BIAS control inoperative with the HW-101; set by the transceiver's BIAS. HP-13 bias fully CW (pp. 113–114). |
| Sensitivity, output, stability | Not stated / 100 W | < 0.3 µV; 100 W on 80–15, 80 W on 10; < 100 Hz/hr after **45 min** (pp. 157–158) |
| 40 m coverage | 7.0–7.5 implied | **7.0–7.3 MHz** |
| V-number table | "Verify" | Confirmed from schematic, block diagram and troubleshooting chart |
| Diodes | Partial, CR201/CR901 only | Full table; CR1–4 are FH-1100 in this printing; D202 15 V zener; D907 not in the manual's table |
| Electrolytics | Ratings guessed (15 V / 350 V) | Five confirmed with p/n; manual prints no voltages; C906 is the **−130 V bias filter** |
| R106 / R107 | 22 kΩ 1 W / 100 kΩ ½ W | 22 kΩ 1 W / **100 kΩ 1 W already** in this printing |
| Power resistors | — | Added R302 3.3 kΩ 1 W (bias), **R304 1 kΩ 7 W / R305 2.5 kΩ 7 W** (0A2 dropping) |
| Bias | "50 mA dot" vs. Latta "3" (conflict) | **▼ mark above "3" = 50 mA resting cathode current**, set in PTT; checked in TUNE before every tune-up (pp. 122, 140) |
| Loading | "~40" | "40" = **250 mA**; full meter scale table (p. 139) |
| Initial test | Generic | Manual Initial Test settings and **+275 V / −108 V** checkpoints; earth-ground warning (p. 117) |
| Alignment | Field summary | Full manual sequence: T103 and T1 "up 14 turns then peak", VFO trimmer preset, neutralizing preset one turn CCW, ALC re-zero at 29.5, calibrator at 15 MHz WWV |
| Receive benchmark | S9+20 → S6 (W5RKL) | Manual: **≥ S9+20 at 3700 kHz → S3 at 29.2 MHz** |
| Troubleshooting | Bulletins only | Added manual chart items (D101, C213, V7, R221, CR941, rotors 180°, 250 V B+) and VFO chart |
| Late-printing features | — | Steel comb brackets, new detents, #47 lamps, FH-1100 modulator diodes already supplied |
