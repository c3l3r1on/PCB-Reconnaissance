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

# TP-Link Archer C1200 v2 PCB-Reconnaissance

Set of PCB photos used during the hardware-level diagnosis of a `TP-Link Archer C1200 v2`.

## Support The Lab
- If you have a bit of dead hardware, half-dead hardware, or "it only smoked once" hardware lying around, I will gladly take it in and turn it into documentation.
- Coffee is also accepted with zero resistance:
  - Ko-fi: <a class="link" href="https://ko-fi.com/c3l3r1on" target="_blank" rel="noopener">ko-fi.com/c3l3r1on</a>
  - Buy Me a Coffee: <a class="link" href="https://buymeacoffee.com/c3l3r1on" target="_blank" rel="noopener">buymeacoffee.com/c3l3r1on</a>

## Markings Read From The Board
- Main Ethernet switch IC: `Broadcom BCM53125SKMMLG TE1734 P30 277-20 3B W`
- Main Broadcom SoC visible under the shield window: `BCM47189B0KRFBG HN1737 P20 408-29 N3A W`
- Crystal near switch area: `Y1`, marked `25.0 S GL`
- Ethernet magnetics branding visible on PCB photos: `HST-36001DR` `GROUP-TEK` `1744B`, `HST-18001DR` `GROUP-TEK` `1738`
- PCB marking visible on board: `KB-6160` `1742` `A MKM` `94V-0` `10558` `E248237`
- PCB / board code visible on board: `2050500863`

## Sticker / Assembly Markings
- Product sticker visible on the board-side assembly photo:
  - `Archer C1200(EU) Ver:2.0`
  - `2BHC184000074`
- Additional assembly / line sticker:
  - `3FA4-03`

## Context
- Firmware was restored to stock.
- Runtime and NVRAM returned to the expected LAN/WAN mapping.
- Despite that, the switch still reported a false `port 0` link even with no Ethernet cable connected.
- The investigation moved from config/runtime into switch/PHY/power territory.

## Useful UART Findings
- CPU / platform reported by UART:
  - `ARMv7`
  - `Hardware: Northstar Prototype`
  - kernel `Linux 2.6.36.4brcmarm`
- Flash layout reported by runtime:
  - `mtd0` `boot` `0x00080000`
  - `mtd1` `linux` `0x00f70000`
  - `mtd2` `rootfs` `0x00d70000`
  - `mtd3` `usb-config` `0x00010000`
  - `mtd4` `log` `0x00020000`
  - `mtd5` `nvram` `0x00010000`
- Full flash / partition-table analysis also showed vendor partitions outside the simple runtime MTD view, including:
  - `default-config`
  - `user-config`
  - `profile`
  - `product-info`
  - `soft-version`
  - `extra-para`
  - `qos-db`
  - `radio`
  - `radio_bk`
- Stock network mapping confirmed over UART after reset:
  - `LAN = eth0.1`
  - `WAN = eth1.4094`
  - `br-lan = 192.168.0.1`
  - `sysmode = router`
- Important diagnostic result:
  - after restoring stock config, `et -i eth0 port_status all` still showed `port 0 Up`
  - this persisted even with no RJ45 cable connected
  - this is the main reason the investigation shifted away from UCI/config and toward hardware
- Factory reset behavior worth noting for other researchers:
  - on this firmware, reset does not mean "rewrite the whole flash"
  - it mainly restores `default-config -> user-config`
  - board-specific data outside that config path can still preserve hardware-specific failure modes
- USB note from UART:
  - `RTL8188EUS` (`0bda:8179`) was physically detected
  - stock firmware bound it to `KC NetUSB General Driver`, not as a normal Wi-Fi interface

## Board-Level Lead
- The main suspect area is the Broadcom switch region and its reference clock `Y1`.
- Close-up shots show `Y1` marked `25.0 S GL`, which matches a `25 MHz` crystal.
- Under the microscope, the crystal itself and its solder joints did not show obvious mechanical damage.
- Resistance probing later showed a very low resistance on the investigated rail in that area, which pushed the diagnosis further toward hardware rather than config.

## Files

### `photos/photo02.jpg`
- Top-side overview of the PCB.
- Good for showing:
  - RJ45 port area and magnetics
  - main heatsink / thermal spreader
  - power section on the right
  - service wire placement

### `photos/photo03.jpg`
- Bottom-side overview of the PCB.
- Useful for:
  - locating measurement points
  - following vias and power distribution
  - orienting the underside relative to the heatsink/shield area

### `photos/photo04.jpg`
- Close-up of the exposed Broadcom area after opening the shielded section.
- Visible in frame:
  - the main switch / Ethernet IC
  - nearby passives
  - local capacitors and resistors around clock/power circuitry

### `photos/photo05.jpg`
- Second close-up of the Broadcom switch area.
- Clearly shows:
  - `BCM53125SKMMLG` `TE1734 P30` `277-20 3B W`
  - crystal `Y1`
  - surrounding passives `R8`, `R102`, `C221`, `C222`
- This was one of the key images used to guide further probing.

### `photos/photo06.jpg`
- Close-up of the lower-right PCB area with service wires.
- Visible in frame:
  - hand-soldered wire area
  - SPI flash `HJ1742` `25Q127CSIG` `AP16207`

### `photos/photo07.jpg`
- Second top-side overview shot.
- Better for quick presentation of:
  - board topology
  - Broadcom switch position relative to the ports
  - power, USB, and DC input sections

### `photos/photo08.jpg`
- Bottom-side overview, newer and cleaner than the earlier underside photo.
- Useful for:
  - general underside documentation
  - locating shield perimeter and passive clusters
  - correlating later close-ups with the full PCB layout

### `photos/photo09.jpg`
- Top-side board shot with the metal heatsink/shield frame in place.
- Useful for:
  - identifying the relative placement of RAM, main Broadcom logic, and the shield windows
  - showing the board state used for later close-up reconnaissance

### `photos/photo10.jpg`
- Close-up of the Broadcom main logic area through the shield window.
- Best used for:
  - reading the package marking on the Broadcom IC
  - documenting the residue / surface condition on the chip package
  - `BCM47189B0KRFBG` `HN1737 P20` `408-29 N3A W`

### `photos/photo11.jpg`
- Close-up of the NANYA memory IC area.
- Useful for:
  - confirming the RAM vendor/package marking
  - documenting the nearby power inductor and local passive network
  - `NT5CC64M16GP-DI` `71905300EP L TW`

### `photos/photo12.jpg`
- Close-up of the upper shield window showing the crystal and nearby support ICs/passives.
- Useful for:
  - documenting the clock/reference area above the Broadcom package
  - correlating the crystal region with later probing work

### `photos/photo13.jpg`
- Very close macro of the small IC in the upper shield window.
- The image is softer than the other close-ups, but it still helps document:
  - package location
  - approximate marking
  - surrounding passives in that sub-area
  - `888 633C 6744`

### `photos/photo14.jpg`
- Macro close-up of the small SOT-23 / power-related sub-area near `U9`.
- Useful for:
  - documenting local support circuitry
  - correlating later resistance/power measurements with exact component placement

### `photos/photo15.jpg`
- Board-side photo showing the product and assembly stickers near the port block.
- Useful for:
  - preserving the exact hardware/version sticker data
  - documenting unit-specific and assembly-line identifiers
  - correlating the photographed PCB with the retail model/version marking

### `photos/photo16.jpeg`
- Macro close-up of the `BCM53125SKMMLG` `TE1734 P30` `277-20 3B W` switch and crystal `Y1`.
- Good publication image because it clearly shows the relationship between:
  - Broadcom switch
  - `Y1 25.0`
  - clock-path passives
  - magnetics / port area in the background

### `photos/photo17.jpeg`
- Thermal frame.
- Shows a hotspot in the Ethernet / switch / front-edge port area.
- The highlighted maximum in this image reaches roughly `73.8 C`.
- This should be treated as a diagnostic clue, not as standalone proof that a specific component is faulty.

## Practical Notes
- For `pcb-resistance` probing, the most useful region was:
  - `BCM53125`
  - `Y1`
  - `C221/C222`
  - decoupling capacitors near the switch
- If this folder is published, a sensible image order is:
  1. top view
  2. bottom view
  3. switch close-up
  4. `Y1` close-up
  5. thermal frame

## Current Working Conclusion
- The photos support the conclusion that the issue is no longer in firmware/configuration alone.
- The strongest lead is hardware in the switch/PHY/power area, not incorrect logical LAN/WAN configuration.
