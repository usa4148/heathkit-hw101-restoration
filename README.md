# Heathkit HW-101 Restoration Plan

A restoration plan for the Heathkit HW-101 HF SSB/CW transceiver (1970–1983) and its HP-23 power supply. It covers which parts fail with age, a phased restoration procedure, alignment and neutralization, a symptom-to-service-bulletin index, and maintenance.

**Status:** Rev. 2 — checked against the original Heath manual (595-1277-18), with Heath service bulletins and field sources (W5RKL, Latta, WB8JKR, K4JPN, the Heath list). See §11 of the plan for the change log.

## Files

- [`hw101-restoration-plan.md`](hw101-restoration-plan.md) — source (pandoc markdown)
- [`hw101-restoration-plan.pdf`](hw101-restoration-plan.pdf) — rendered PDF

## Build

```sh
pandoc hw101-restoration-plan.md -o hw101-restoration-plan.pdf \
  --pdf-engine=xelatex -V mainfont="DejaVu Serif"
```

## Safety

The HP-23 supplies about 700–800 V DC, which stays on the filter capacitors after unplugging. Discharge them before working inside.

## Sources

- Heath HW-101 manual, service bulletins and HP-23 manual: Bill Latta's HW-101 site (frostburg.edu)
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*
- WB8JKR, *Optimizing the HW-101, SB100-102 Transceivers*; K4JPN, *Improve Your HW-101*
- Heath mailing list archive and the Heathkit-Radios group on groups.io

See also: [heathkit-sb301-restoration](https://github.com/usa4148/heathkit-sb301-restoration)
