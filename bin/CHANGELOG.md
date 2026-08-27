# 📜 SVA-Fencing-Tester – Version History & Changelog

This file documents the release history, firmware builds, and corresponding webapp/LittleFS updates for the SVA-Fencing-Tester initiative.

---

## 🛠️ Current Development Phase (Under Construction)

### [0.7.0+71] - 2026-08-17
* **Firmware (ESP32):**
  * empty test file
* **Webapp & LittleFS:**
  * empty test file
* **Testing:**
  * test repo connection

---

## 📋 Release Format & Versioning Rules

We use a specific versioning structure tailored to our automated pre- and post-build system:
`SVA-Fencing-Tester.[BRANCH]_[MAJOR].[MINOR].[PATCH]+[BUILD]_[YYYY-MM-DD]_[HH]_[MM].ota`

* **Format Type:** Combined All-in-One Bundle (contains both the compiled firmware partition and the LittleFS image).
* **Delivery:** Clients connect to the offline device Access Point, fetch the `version.json` from this repository via their active internet connection, and download the `.ota` package to flash the device locally.

---

## 🛡️ Failsafe & Recovery Matrix

If an update fails or the file system gets corrupted, refer to the following version states:

| Firmware Version | Required Webapp | LittleFS Layout | Minimum Recovery Version |
| :--- | :--- | :--- | :--- |
| **0.7.0+71** | webapp.gz (v0.7) | Layout V1 (Lite Manual) | v0.5.0 (Recovery Server Base) |
