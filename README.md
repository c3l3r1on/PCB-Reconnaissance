# PCB-Reconnaissance

Hardware teardown notes, board photography, and component identification for small embedded devices.

The goal of this repository is not to produce glossy marketing writeups. It is to build practical reconnaissance artifacts that help with:

- board-level reverse engineering
- connector and power-rail mapping
- component identification from teardown photos
- bringing up unknown devices for debugging or repair

## Repository Layout

```text
Hardware-Teardowns/
  <vendor-or-device>/
    README.md
    photos/
```

Each teardown should separate:

- `Confirmed` observations: visible markings, connectors, silkscreen, board IDs
- `Inferred` observations: likely function based on topology, device class, and board layout
- `Unknowns`: items that need better macro photos, continuity checks, or trace following

## Current Entries

- [FORCE IP-V-4025D](/home/c3l/projects/PCB-Reconnaissance/Hardware-Teardowns/FORCE-IP-V-4025D/README.md)
  PoE dome IP camera teardown with a Fullhan `FH8656`, XMC `XM25QH128C` flash, `GC4653`-marked camera board, and `SD4952B` PoE daughterboard.

- [YIZHAN XVH2003](/home/c3l/projects/PCB-Reconnaissance/Hardware-Teardowns/YIZHAN-XVH2003/README.md)
  HDMI/VGA industrial camera teardown with sensor board, main ISP board, and front I/O board notes.

## Documentation Style

The preferred style for this repo is:

- technical and compact
- explicit about confidence level
- useful for follow-up hardware work
- honest about uncertainty when chip markings are not readable

Where a package marking cannot be read from the available photos, the README should name the function and clearly mark the part number as tentative instead of pretending certainty.
