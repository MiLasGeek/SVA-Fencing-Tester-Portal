# 📜 Regulatory Compliance, Specifications & Impressum (Rev 2 & Rev 4) — Draft

> **Status: Entwurf / nicht finale Konformitätsdokumentation.** Dieses Dokument beschreibt den derzeitigen Planungs- und Entwicklungsstand. Es ist **keine** unterschriebene EU-Konformitätserklärung, kein CE-Nachweis, kein CRA-Konformitätsnachweis und kein FIE-Zertifikat. Finale, datierte und gegebenenfalls unterzeichnete Unterlagen werden nach Abschluss der technischen Prüfung und Freigabe veröffentlicht.

The SVA-Fencing-Tester (supporting hardware revisions 2 and 4 via automatic firmware detection) is intended to be used exclusively as a specialized workshop diagnostic and maintenance tool for fencing gear. It is not intended as a tournament signaling apparatus (Melder). The final regulatory classification and the scope of any required assessment will be confirmed before product release.

---

## 🤺 1. FIE Reference & Specifications — Draft
The intended use is a dedicated workshop test bench for fencing equipment. It is not a tournament scoring apparatus (Melder) and does not display or control competition hits. The project aims to document conformity of the tested fencing material with the applicable FIE material requirements. A separate FIE certification of the tester itself is not intended for this diagnostic use; the final scope will be documented before product release.

* **Signal Evaluation:** Designed to test and analyze weapon assemblies, body cords, and reels against the applicable electrical tolerance windows in the FIE Technical Rules.
* **Microsecond Analytics:** The firmware loop records transient resistance changes in the microsecond range to visually isolate mechanical contact bouncing (Prellen) before it triggers a lockout on an official piste scoring box.
* **Pure Diagnostic Scope:** The unit does not actively interfere with tournament scoring loops. It serves solely as a diagnostic aid for material inspection before use.

---

## 🇪🇺 2. CE / RED / RoHS — Planning Status
The following points define the intended compliance scope for the final device. They do not constitute a declaration that either revision already satisfies the listed directives:
* **Radio Equipment Directive (RED - 2014/53/EU):** Planned assessment of the local 2.4 GHz Wi-Fi function and transmission parameters.
* **EMC Directive (2014/30/EU):** Planned assessment of electromagnetic compatibility based on the finalized hardware design.
* **RoHS Directive (2011/65/EU):** Component and supplier documentation will be collected for the final bill of materials.

---

## 🔒 3. EU Cyber Resilience Act (CRA) — Preliminary Security Concept
The SVA-Fencing-Tester (Rev 2 / Rev 4) is being developed according to "Security by Design" principles. The following describes the intended architecture and must be validated against the final hardware, firmware, update process and applicable CRA requirements:

* **Zero Personal Data (GDPR Free):** The system does not collect, process, or store any personal user data or fencing club metrics.
* **Credential Hardening:** Local Wi-Fi credentials and security tokens are securely hashed/encrypted within the non-volatile storage and cannot be extracted via the user interface or wireless dumps.
* **Secure Configuration Backup:** Parameter and JSON configuration files are encrypted during export. While the local import logic accepts standard schemas for club flexibility, unencrypted exploitation from the outside is blocked.
* **Minimized Attack Surface (No OS / No FTP):** The device does not run a generic operating system (No OS overhead). High-risk legacy protocols like FTP, Telnet, or SSH are completely omitted.
* **No Internet Routing:** The device operates exclusively as a local, air-gapped server. It features no network forwarding, no internet gateway routing, and cannot connect to external cloud services.
* **Hardened Server & Binary Streams:** The local network architecture consists solely of a locked-down, proprietary server utilizing a minimalist REST API and an isolated, purely binary measurement data stream for real-time hit telemetry.
* **Strict Upload Restriction:** Outside of the authenticated OTA firmware update pipeline and the local configuration JSON import, no files can be uploaded or written to the internal LittleFS partition.
* **Physical Service Boundary:** Secure Boot is not enabled. Flash encryption and the custom OTA format protect the confidentiality of release contents and the regular update paths, but they do not prevent physical reprogramming. Reprogramming requires opening the device, the custom programming adapter and an independently developed firmware for the custom hardware; a device modified in this way is outside the released and supported configuration.

---

## 📦 Proprietary Encrypted OTA Bundle Format (SVA-Container)

To protect release contents and maintain a controlled update workflow, the SVA-Fencing-Tester uses an encrypted, proprietary bundle format (`.ota`).

Unencrypted firmware binaries (`.bin`) or raw file system images are never distributed publicly and will be automatically rejected by the device's bootloader.

### 🛡️ Core Security Layers of the `.ota` Container:

1. **Cryptographic Encapsulation (Container Encryption):**
   The firmware payload and LittleFS system assets (webapp and lite manual) are packed into a proprietary container using a private cryptographic routine prior to deployment on the `main` branch. The intended protection against extraction or unauthorised modification depends on the implemented cryptography, key management and final release configuration.
2. **Integrity & Origin Validation:**
   Before executing any flash sequence, the device validates the structure, header signatures, and checksums of the custom bundle. Modified, corrupted, or third-party tampered files are rejected by the main updater and recovery server.
3. **Intellectual Property Protection:**
   This closed distribution workflow is intended to protect the integrity of released update bundles and the project's proprietary implementation.

---

*Note for the Testing Phase: The files deployed to the public `main` branch during the current 'Under Construction' phase contain purely non-functional dummy datasets to validate the download, storage routing, and parsing logic of the device webapp.*

---

## 📜 Impressum & Anbieterkennzeichnung (Legal Notice)

Dieses Portal ist eine private, ehrenamtliche Non-Profit-Initiative zur Unterstützung des Fechtsports.

Die Anbieterkennzeichnung und Kontaktangaben sind im zentralen [Impressum](../../IMPRINT.md) als Grafik hinterlegt.

*For official compliance inquiries or full hardware verification requests, please refer to the contact details provided in the integrated 'About' page of the official device webapp.*
