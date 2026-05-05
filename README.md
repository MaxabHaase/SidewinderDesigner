# Sidewinder v2 Designer

A local design toolkit for **Sidewinder DNA assembly**.

This project packages a workflow for turning a desired DNA construct into a set of oligos for Sidewinder assembly, with a focus on repeat-rich targets such as **lacO arrays**, plus a separate visualization module for inspecting pairing logic across fragments.

## What the toolkit does

The project currently includes two main scripts and one visualization helper:

- `sidewinder_v2_designer.py` — backend design engine
- `sidewinder_v2_local_ui.py` — local Streamlit UI
- `sidewinder_v2_visualizer.py` — separate visualization tool for assembly maps

The designer can:

- design Sidewinder oligos for a **generic target sequence**
- design Sidewinder oligos for a **repeat array**
- choose fragment partitions under Sidewinder-specific oligo constraints
- assign **toeholds** and **barcodes**
- design **heuristic barcodes** or optionally use **NUPACK**
- report final construct sequence, oligo tables, JSON metadata, and FASTA outputs
- expose terminal ssDNA tails on template oligos for primer binding when long flanks are supplied

The UI adds:

- single-design mode
- batch repeat-array mode
- downloadable ZIP outputs
- progress reporting during design
- optional NUPACK settings when a local NUPACK install is detected

The visualizer adds:

- assembly overview SVGs
- junction-detail SVGs
- color-coded barcode and toehold pairing maps

## Why this exists

Sidewinder is powerful for difficult constructs because the assembly-guiding sequences are separated from the final desired product sequence. That makes it attractive for repetitive and otherwise difficult targets, but it also makes manual design burdensome.

The practical bottleneck is not just the chemistry. It is also:

- fragment partitioning
- toehold placement
- barcode selection
- strand geometry
- terminal primer / exposed-tail handling

This toolkit is meant to make that design process accessible and reproducible in a local lab setting.

## Design model implemented in v2

The v2 backend uses the corrected strand model:

- **barcode oligo** carries the **product strand**
- **template oligo** carries the **complementary strand**
- **toeholds are part of the final desired sequence**
- cross-fragment annealing is represented explicitly

For internal fragment `i`:

- barcode oligo: `rc(b(i-1)) + x(i) + t(i) + b(i)`
- template oligo: `rc(x(i)) + rc(t(i-1))`

with the expected first/last fragment edge cases.

## Main outputs

A typical design run produces:

- CSV order sheet
- JSON design report
- FASTA sequence export

The JSON report is also the preferred input for the separate visualization tool.

## Requirements

- Python 3.10+
- Streamlit for the UI
- pandas for tabular export
- optional: **NUPACK 4** for thermodynamic barcode design

## Running locally

### Backend only

```bash
python sidewinder_v2_designer.py --help
```

### UI

```bash
python -m pip install -r requirements_sidewinder_v2_ui.txt
streamlit run sidewinder_v2_local_ui.py
```

### Visualizer

```bash
python sidewinder_v2_visualizer.py \
  --json your_design.json \
  --out-prefix sidewinder_vis
```

## Recommended repository layout

```text
sidewinder-v2/
├── sidewinder_v2_designer.py
├── sidewinder_v2_local_ui.py
├── sidewinder_v2_visualizer.py
├── requirements_sidewinder_v2_ui.txt
├── README.md
└── docs/
    ├── index.md
    └── _config.yml
```

## Suggested citation / project description

**Sidewinder v2 Designer** is a local software workflow for designing oligos for Sidewinder DNA assembly, including repeat-array design, optional NUPACK-based barcode design, and an independent visualization module for inspecting barcode, toehold, and fragment pairing relationships.

## Status

This project is a working local design toolkit. It is best treated as a design and planning system for Sidewinder assemblies rather than a claim of fully automated experimental validation.
