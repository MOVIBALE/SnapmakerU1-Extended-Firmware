# Snapmaker U1 Mixed Nozzle Validation Patch

This patch is intended to pair with the experimental Snapmaker Orca
`Min/2.3.5-beta-mixed-nozzle` branch.

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

## Building And Distribution

Build with the repository's documented Docker workflow:

```sh
./dev.sh make build PROFILE=extended OUTPUT_FILE=firmware/U1_extended_mixed_nozzle.bin
```

This repository and the patched Klipper component are GPL-3.0. A distributed
binary must identify the exact source commit, keep the license notices, and
provide recipients access to the corresponding source. Do not publish an old
local binary as if it were built from a newer commit.

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

To restore official behavior, keep a known-good official U1 image before
flashing and follow the recovery/reflash procedure appropriate for the
currently installed extended firmware. This is an independent community patch,
not an official Snapmaker firmware release.

The ESP32 Timelapse Box is unrelated: it uses the slicer's
`ESP_TIMELAPSE_SHOT` Klipper macro boundary and does not require this patch.
