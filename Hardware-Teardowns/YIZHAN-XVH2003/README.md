# 🛰️ PCB-Reconnaissance: YIZHAN XVH2003 Teardown

## 🔍 The Main Hero (Full Internal View)
Detailed hardware analysis of the YIZHAN XVH2003 HDMI/VGA Industrial Camera.

![Main Hero - PCB Overview](photos/main_hero.jpg)

---

## 🛠️ Mainboard Architecture
### Top View & SoC
The heart of the device is the Fullhan Image Signal Processor. Signal routing is dense, optimized for real-time 1080p scaling.

| Main Board (Top) | SoC Macro (Fullhan) |
| :---: | :---: |
| ![Board Top](photos/main_board_top.jpg) | ![SoC Macro](photos/soc_macro.jpg) |

### Bottom Board & Power
The secondary layer handles power regulation and sensor interfacing.

![Bottom Board](photos/bottom_board.jpg)

---

## 🔌 Interfaces & Enclosure
### I/O Panel & Controls
Integrated buttons for OSD menu management and physical output ports (VGA/HDMI).

| I/O Board & Buttons | Enclosure Port Cover |
| :---: | :---: |
| ![IO Controls](photos/io_panel_controls.jpg) | ![IO Cover](photos/enclosure_io_cover.jpg) |

### The "Eye" (Sensor)
Featuring the GalaxyCore GC2053 sensor, providing the high-definition feed for the entire pipeline.

![GalaxyCore Sensor](photos/sensor_galaxycore.jpg)

---

## 🔬 Microscope Recon (DRO)
Digital microscopic analysis of critical signal points.

| Sensor Pads | ISP Traces | PCB Labels |
| :---: | :---: | :---: |
| ![Micro 1](photos/micro_sensor_pads.png) | ![Micro 2](photos/micro_isp_traces.png) | ![Micro 3](photos/micro_labels.png) |

---

## ⚖️ Safe Harbor
Educational hardware reconnaissance by **Lab t4rg3d**. 
Crack the Planet. If you can't open it, you don't own it.

