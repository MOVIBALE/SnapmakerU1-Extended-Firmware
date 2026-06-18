# Snapmaker U1 Mixed Nozzle Validation Patch

This patch is intended to pair with the experimental Snapmaker Orca
`mixed-nozzle-u1` branch.

## Problem

The stock U1 print task validation compares all used physical toolheads against
the first nozzle diameter reported by the slicer. That rejects jobs where a
logical tool uses a different nozzle diameter, even if the physical toolhead is
correct.

Example:

- logical T0: 0.4 mm
- logical T1: 0.2 mm
- `extruder_map_table = [1, 0, 2, 3]`

The firmware must compare each used logical tool against the physical toolhead
selected by `extruder_map_table`, not against `nozzle_diameter[0]`.

## Patch

The overlay patch is:

`overlays/firmware-extended/11-patch-klipper/patches/home/lava/klipper/09_mixed_nozzle_validation.patch`

It updates `klippy/extras/print_task_config.py` so validation uses:

`logical_index -> extruder_map_table[logical_index] -> actual_nozzle_diameter[physical_index]`

Unused logical tools are skipped based on both `filament_used_g` and
`filament_used_mm`.

## Scope

This patch only changes nozzle-diameter validation. It does not change:

- motion planning
- extrusion calibration
- pressure advance
- purge or wipe behavior
- tool offset calibration
- bed probing
- print recovery behavior

## Local Artifact

Current local test build:

`firmware/U1_extended_1.4.1-paxx12-19_mixed-nozzle-codex.bin`

## Real Print Validation

A real Snapmaker U1 mixed-nozzle test print completed successfully on
2026-06-18 using the patched slicer and this validation patch.

Validation photo:

`https://raw.githubusercontent.com/MOVIBALE/OrcaSlicer/mixed-nozzle-u1/docs/mixed-nozzle-u1/assets/real-print-cube.jpg`

## Risk

This is experimental firmware. Flash only if you have a known-good recovery
image and are comfortable restoring the printer.

Print quality and safety still depend on correct slicer settings, physical
nozzle installation, center-to-center tool offset calibration, purge behavior,
and first-layer tuning.
