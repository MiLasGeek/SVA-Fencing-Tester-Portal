# 🛠️ Bill of Materials (BOM) & Spare Parts Specification

The SVA-Fencing-Tester utilizes a hybrid repair philosophy to balance high-end electronic reliability with maintenance accessibility for fencing clubs.

## ⚠️ High-Density SMD Warning (Main PCB)
The Core Carrier PCB is a highly integrated, custom **surface-mount device (SMD)** assembly. The ESP32 module is soldered directly onto the board with a hidden thermal ground pad underneath. 
* **No Field Rework:** Individual electronic components, chips, and the MCU itself **cannot be replaced by hand or with standard soldering irons** at the club.
* **Component Replacement:** In the event of a core hardware failure, the entire pre-assembled PCB must be swapped out, or the device must be sent back for specialized volunteer repair.

---

## 📐 Core Components & Cost Matrix

All components are purchased with regular gross taxes included from official distributors (Mouser, Aisler). Prices reflect absolute non-profit material procurement costs.

| Component | Specification / Type | Function / Integration | Primary Source | Club Replaceable? | Approx. Cost |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Main PCB Assembly** | SVA-Fencing-Tester V1.x (Custom SMD) | Core routing, power management & embedded ESP32 module | [Aisler](https://aisler.net) / Pre-Assembled | **No** (Swap entire board only) | **~150.00 €** |
| **OLED Display** | 2.42" Module (SPI / I2C Variant) | Local UI & target hit visualization | [Mouser](https://mouser.com) | **Yes** (Via modular connector) | *Market price* |
| **Battery Pack** | LiPo 3.7V (with integrated PCM circuit) | Mobile power supply using a **JST-connector** | Reliable Battery Vendor | **Yes** (Plug & Play - No soldering) | *Market price* |
| **Fencing Sockets** | Three-Pin Sockets (Standard FIE) | Weapon & Reel hardware interface | Fencing Suppliers | **Yes** (Via screw terminals) | *Market price* |

---

## 📦 Mechanical & 3D-Printed Parts

Structural parts are 100% user-replaceable. Files are located in the `/hardware` directory of this portal.
* **Basic Enclosure:** Shell for everyday club use. Available in `/hardware/housing-basic/`.
* **Wear Parts:** High-stress elements like button caps, enclosure clips, and protective bumpers. Available in `/hardware/spare-parts/`.

---

## 🔧 Repair & Service Strategy for Clubs

1. **Modular Self-Service:** Fencing clubs are highly encouraged to perform simple swaps themselves (replacing the display, changing the battery pack via JST plug, or 3D-printing worn-out plastic parts).
2. **PCB Failure Support:** If the main electronics fail, do not attempt to desolder the ESP32. Please open a [Hardware Request Ticket](../../issues) to coordinate either a full board replacement (~150€ material cost) or an ehrenamt-level repair request depending on volunteer availability.
