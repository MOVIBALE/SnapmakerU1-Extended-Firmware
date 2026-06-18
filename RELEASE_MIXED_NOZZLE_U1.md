# GitHub Release Draft: U1 mixed-nozzle validation firmware

## Title

Snapmaker U1 extended firmware mixed-nozzle validation test build

## Tag

`mixed-nozzle-u1-v0.1.0`

## Target Branch

`mixed-nozzle-u1`

## Summary

Experimental Snapmaker U1 extended firmware build for testing mixed-nozzle
G-code produced by the matching Snapmaker Orca `mixed-nozzle-u1` branch.

This release changes U1 print task nozzle validation so each used logical tool
is compared with the physical nozzle selected by `extruder_map_table`.

## Asset To Attach

`firmware/U1_extended_1.4.1-paxx12-19_mixed-nozzle-codex.bin`

## Validation

- Patch added as an overlay patch under
  `overlays/firmware-extended/11-patch-klipper/patches/home/lava/klipper/`.
- Firmware `.bin` exists locally.
- User flashed the generated firmware and the printer booted.
- Real Snapmaker U1 mixed-nozzle print on 2026-06-18 passed.

Real print photo:

`https://raw.githubusercontent.com/MOVIBALE/OrcaSlicer/mixed-nozzle-u1/docs/mixed-nozzle-u1/assets/real-print-cube.jpg`

## Warning

This is experimental firmware. It only changes validation logic and does not
guarantee safe print behavior for all nozzle/material combinations.

Keep a known-good firmware image ready before flashing. Stop the first test
print immediately if tool offsets, first-layer contact, purge, or extrusion look
wrong.

## Known Limitations

- Does not tune purge, wipe, or pressure advance.
- Does not change tool offset calibration behavior.
- Does not validate mixed layer heights.
- Requires matching slicer G-code that reports per-logical-tool nozzle
  diameters.
