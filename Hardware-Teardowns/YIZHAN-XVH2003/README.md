# 🛰️ YIZHAN XVH2003 HDMI/VGA Microscope Camera – Teardown & PCB Analysis

## 🔍 Overview
- **Model:** YIZHAN XVH2003
- **Sensor:** GC2053 (1/3")
- **Output:** HDMI / VGA (1080p)
- **Marketing:** "13MP" (interpolated)

![PCB Front - Fullhan ISP](photos/pcb_front_isp.jpg)

## 🛠️ Mainboard Identification
- **Main PCB:** ZS_ISP262_HVUA_DIP30_V2.1
- **I/O Board:** ZS_6kay_HV_DIP30_V2.1

![I/O Panel and OSD Buttons](photos/io_panel_buttons.jpg)

## 🧠 SoC and Signals
The core logic is handled by a Fullhan 8788-EX ISP. 
Detailed inspection of the HDMI section reveals HPD (Hot Plug Detect) signals and dedicated 3.3V rails.

![HDMI Section Detail](photos/hdmi_section_detail.jpg)

## 📡 Sensor Interface
The sensor connects via a 30-pin FPC/DIP interface. Parallel CMOS signals (PCLK, DATA) are routed directly to the ISP.

![Sensor Connector Detail](photos/sensor_connector_detail.jpg)

---
*Part of the PCB-Reconnaissance project by c3l3r1on.*
