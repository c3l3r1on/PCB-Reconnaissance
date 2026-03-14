In the Name of Root
and Saint UART.

Ohmmmm, Brothers in the Circuit.

May the Resistor Be With You.
Burn, Dump, Repeat.
Glory to PCB and SMD.

For firmware is written,
but hardware is eternal.

And remember:

We Are All Grounds Here.

# PCB-Reconnaissance

Hardware teardown notes, board photography, and component identification for small embedded devices.

The goal of this repository is not to produce glossy marketing writeups. It is to build practical reconnaissance artifacts that help with:

- board-level reverse engineering
- connector and power-rail mapping
- component identification from teardown photos
- bringing up unknown devices for debugging or repair

## Support The Lab

- If you have a bit of dead hardware, half-dead hardware, or something that "only smoked once", I will gladly take it in and turn it into documentation.
- Coffee is also accepted with zero resistance:
  - Ko-fi: <a class="link" href="https://ko-fi.com/c3l3r1on" target="_blank" rel="noopener">ko-fi.com/c3l3r1on</a> <a class="link" href="https://ko-fi.com/c3l3r1on" target="_blank" rel="noopener"><img src="https://cdn.simpleicons.org/kofi/00D4FF" alt="Ko-fi"></a>
  - Buy Me a Coffee: <a class="link" href="https://buymeacoffee.com/c3l3r1on" target="_blank" rel="noopener">buymeacoffee.com/c3l3r1on</a> <a class="link" href="https://buymeacoffee.com/c3l3r1on" target="_blank" rel="noopener"><img src="https://cdn.simpleicons.org/buymeacoffee/00D4FF" alt="Buy Me a Coffee"></a>

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

- [FORCE IP-V-4025D](Hardware-Teardowns/FORCE-IP-V-4025D/README.md)
  PoE dome IP camera teardown with a Fullhan `FH8656`, XMC `XM25QH128C` flash, `GC4653`-marked camera board, and `SD4952B` PoE daughterboard.

- [TP-Link Archer C1200 v2](Hardware-Teardowns/TP-Link-Archer-C1200-v2/README.md)
  Router PCB reconnaissance focused on a persistent Broadcom switch false-link fault, stock-firmware recovery, UART findings, and board-level troubleshooting around the `BCM53125` and its `25 MHz` reference clock. Polish companion: [README_PL](Hardware-Teardowns/TP-Link-Archer-C1200-v2/README_PL.md).

- [YIZHAN XVH2003](Hardware-Teardowns/YIZHAN-XVH2003/README.md)
  HDMI/VGA industrial camera teardown with sensor board, main ISP board, and front I/O board notes.

## Documentation Style

The preferred style for this repo is:

- technical and compact
- explicit about confidence level
- useful for follow-up hardware work
- honest about uncertainty when chip markings are not readable

Where a package marking cannot be read from the available photos, the README should name the function and clearly mark the part number as tentative instead of pretending certainty.
