# 🏷️ Firmware Naming & Versioning Convention

To ensure full traceability across our automated pre- and post-build systems, all official binary bundles deployed to the `main` branch follow a strict naming matrix.

## 📐 Filename Pattern
`SVA-Fencing-Tester.release_[MAJOR].[MINOR].[PATCH]+[BUILD]_[YYYY-MM-DD]_[HH]_[MM].ota`

### Breakdown of the Matrix Components:
* **`SVA-Fencing-Tester.release_`**: Fixed prefix identifying official production/testing distribution builds.
* **`[MAJOR]`**: Incremented for groundbreaking changes (e.g., complete UI overhaul, structural breaking changes in the communication protocol).
* **`[MINOR]`**: Incremented for new feature additions (e.g., adding a new fencing tournament mode to the webapp).
* **`[PATCH]`**: Incremented for backward-compatible bug fixes (e.g., UI alignment fixes or battery calibration tweaks).
* **`+[BUILD]`**: The absolute build counter from the automated build pipeline. Essential for distinguishing multiple compilations within the same patch level.
* **`_[YYYY-MM-DD]`**: The exact compilation date (Year-Month-Day).
* **`_[HH]_[MM]`**: The exact compilation time (Hour-Minute) in 24h format.

---

## 🔍 Practical Example
`SVA-Fencing-Tester.release_0.7.0+71_2026-08-17_17_31.ota`

* **Version:** 0.7.0
* **Build Number:** 71
* **Date of Compilation:** August 17, 2026
* **Time of Compilation:** 17:31 (5:31 PM)

## 🤖 Webapp & Device Parsing Logic
The internal updater firmware and the client-side browser bridge use the `+` delimiter to split the active core version string from the build metadata. When the client-side browser bridge fetches the `version.json` from the `main` branch, it evaluates:
1. Is the `[MAJOR].[MINOR].[PATCH]` string higher than the flashed version?
2. If versions match, is the `[BUILD]` number higher than the current device state?

This guarantees that incremental test builds can be pushed and evaluated by test devices without needing to bump the main release version prematurely.
