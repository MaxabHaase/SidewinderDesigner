---
layout: default
title: Sidewinder v2 Designer
---

# Sidewinder v2 Designer

A local toolkit for designing oligos for **Sidewinder DNA assembly**.

## Overview

Sidewinder v2 Designer converts a desired DNA construct into a set of oligos for Sidewinder assembly.

It currently supports:

- generic final-sequence design
- repeat-array design
- optional NUPACK barcode design
- batch design in a local UI
- a separate visualization tool for barcode, toehold, and fragment pairing

## Why it is useful

Sidewinder is attractive for difficult constructs because assembly-guiding sequences can be separated from the final product sequence. That makes repetitive targets and structured targets more tractable, but it also makes the design space much harder to manage by hand.

This toolkit helps with:

- fragment partitioning
- barcode assignment
- toehold placement
- repeat-array planning
- terminal exposed-tail / primer handling
- export of ready-to-review oligo tables

## Core scripts

### `sidewinder_v2_designer.py`
Backend design engine.

### `sidewinder_v2_local_ui.py`
Local Streamlit interface.

### `sidewinder_v2_visualizer.py`
Independent visualization module for assembly maps and junction detail views.

## Typical workflow

1. Enter a target sequence or repeat-array specification.
2. Run the designer.
3. Review the CSV / JSON / FASTA outputs.
4. Use the visualization tool to inspect fragment pairing, barcode pairing, and toehold pairing.

## Design model

The v2 model treats:

- barcode oligos as the **product-strand carriers**
- template oligos as the **complementary strands**
- toeholds as part of the **final desired sequence**

This makes the oligo geometry explicit and closer to the intended Sidewinder assembly logic.

## Running locally

```bash
python -m pip install -r requirements_sidewinder_v2_ui.txt
streamlit run sidewinder_v2_local_ui.py
```

## Visualizer

```bash
python sidewinder_v2_visualizer.py --json your_design.json --out-prefix sidewinder_vis
```

## Outputs

- CSV order sheet
- JSON design report
- FASTA sequence export
- SVG assembly overview
- SVG junction detail map

## Notes

NUPACK is optional and is only used when installed locally and explicitly selected.
