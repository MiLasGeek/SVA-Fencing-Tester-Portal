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

### Evaluation phase

Until the final conformity assessment is complete, non-CE-marked units are supplied only as custom-built evaluation kits to named fencing clubs with a qualified project lead. The club independently procures and connects a suitable battery through the specified connector as part of final assembly, and uses the kit solely in a documented research and development environment. The project may provide a technical battery recommendation, but the battery is not part of the evaluation kit. Battery connection must follow the kit instructions and is not, by itself, a substitute for the required conformity assessment. Private fencers do not receive units directly; any involvement is exclusively through their club within that documented project. This is not a general market release or a regular club operation.

The technical preparation for laboratory testing is complete. Because of the small quantity, laboratory costs are expected to be approximately four to five times the cost of ten devices. Donations and grants are therefore reserved primarily for the required EMC and radio testing.

### Market release gate

No CE declaration will be signed and no CE marking will be affixed before the final device has a documented conformity assessment. For this project, a suitable EMC and radio laboratory report is a required release record. The report, technical documentation, risk assessment, instructions, manufacturer identification, and EU declaration of conformity must be complete and internally approved before any general market release.

Before market release in Germany, the project must also determine and complete all applicable producer obligations for electrical equipment, batteries and packaging, including registration, marking, take-back and disposal information. Final assembly by a club or separate procurement of a recommended battery does not by itself remove these obligations. The evaluation-kit boundary remains subject to documented case-by-case legal review.

For perspective, the published stiftung ear B2B example for one registration currently shows approximately EUR 28.40 net one-off and EUR 131.20 net per year, before any actual disposal costs. For a small run of around twenty devices, this is a material fixed-cost share. The limited evaluation phase is intended to let clubs test the equipment affordably while evidence is gathered for the final release. It does not guarantee exemption from market, CE, disposal, competition or other legal requirements; each supply must remain documented and be assessed for its specific circumstances.

This boundary is not intended to avoid statutory duties. If a supply is classified as making the product available on the market, the applicable duties will be fulfilled regardless of quantity or cost recovery. For a one-off small run, however, recurring registration and administration costs would be a material private subsidy because clubs receive units only at cost. The evaluation phase therefore provides a documented way to establish actual demand and technical maturity before a general release is considered.

A small number of documented units in the field is appropriate for that evaluation; gradual expansion through additional small runs is not. If demand exceeds this limited scope, an appropriate manufacturer should take over professional or automated manufacture, completed conformity assessment and the full market-release process. This is conditional on a price that remains appropriate for clubs and respects the voluntary nature of the project's development.

---

## 🔒 3. EU Cyber Resilience Act (CRA) — Preliminary Security Concept
The SVA-Fencing-Tester (Rev 2 / Rev 4) is being developed according to "Security by Design" principles. The following describes the intended architecture and must be validated against the final hardware, firmware, update process and applicable CRA requirements:

* **Data Minimization / No Tag Database:** The system does not collect, process, or store personal user data, fencing-club metrics, user profiles, or a database of RFID tags. A device loss therefore primarily represents the loss of the physical tester, rather than a disclosure of personal or membership data.
* **Flash Encryption & Release-Content Confidentiality:** The ESP32 release configuration uses Flash Encryption to protect firmware and compiled secrets against direct flash readout. Together with the encrypted proprietary OTA container, it protects release contents at rest and during the controlled update workflow. Secure Boot is not part of the current product profile.
* **Runtime Key Isolation and External RAM:** Cryptographic key material is held only in internal SRAM during its required lifetime and is not intentionally copied into PSRAM, JSON objects, logs, exports or persistent records. Larger transient payloads, such as web/API JSON buffers, may use PSRAM. Where the target enables external-memory encryption, transfers to and from external PSRAM are transparently encrypted; this protects the memory bus and a physical PSRAM read-out, but does not turn PSRAM into a key store.
* **Production Debug Lockdown:** The release-provisioning profile permanently disables unused pad-JTAG and USB-JTAG interfaces. Together with Flash Encryption, this raises the effort required to extract release code or transient secrets. This boundary is intended to resist ordinary physical access; it is not a claim of resistance to invasive laboratory attacks.
* **FRAM-to-ESP Binding / Cloning Resistance:** During provisioning, the technical FRAM identity is cryptographically bound to the immutable ESP eFuse MAC. A random salt and a private HMAC-based binding tag are stored instead of the raw MAC; the binding key is neither stored in FRAM nor published. A copied FRAM record therefore does not pass normal startup on a different ESP. This binding is an integrity and cloning-resistance control, not a substitute for physical tamper protection.
* **Identifier Handling:** The device MAC is not retained as a raw identifier in FRAM. The salted binding tag allows the firmware to verify the hardware association without making an FRAM dump self-explanatory.
* **Wi-Fi Credentials and Configuration Export:** Wi-Fi passwords are not displayed by the device and are not emitted as plain values in a standard configuration export. Credential-bearing exports are protected separately from the persistent FRAM protection and this export protection is not represented as a substitute for authenticated storage encryption. Network names (SSIDs) are not treated as credentials and may remain visible where they are required to identify configured or available networks.
* **Local Device Access Point:** The device's own Wi-Fi access point is an intentional local-service interface for commissioning and use of the web application. Its network name is necessarily discoverable in radio range; access credentials remain protected and are not disclosed through the normal user interface or standard configuration export.
* **FRAM Parameter Encryption:** Persistent parameters in external FRAM use device-bound, authenticated encryption. CRC-protected alternating records additionally preserve integrity and power-loss resilience. The login feature and all functions that change protected settings are coupled to this storage protection.
* **Minimized Attack Surface (No OS / No FTP):** The device does not run a generic operating system (No OS overhead). High-risk legacy protocols like FTP, Telnet, or SSH are completely omitted.
* **No Internet Routing:** The device operates exclusively as a local, air-gapped server. It features no network forwarding, no internet gateway routing, and cannot connect to external cloud services.
* **Hardened Server & Binary Streams:** The local network architecture consists solely of a locked-down, proprietary server utilizing a minimalist REST API and an isolated, purely binary measurement data stream for real-time hit telemetry.
* **Strict Upload Restriction:** Outside of the authenticated OTA firmware update pipeline and the local configuration JSON import, no files can be uploaded or written to the internal LittleFS partition.
* **Physical Service Boundary:** Secure Boot is not enabled. Flash encryption and the custom OTA format protect the confidentiality of release contents and the regular update paths, but they do not prevent physical reprogramming. Reprogramming requires opening the device, the custom programming adapter and an independently developed firmware for the custom hardware; a device modified in this way is outside the released and supported configuration.

### Preliminary security risk treatment

This is a working risk assessment for the released product profile, not a final CRA risk analysis. It will be reviewed with the final firmware, hardware and test evidence before release.

| Scenario | Foreseeable impact | Design treatment and residual limitation |
| --- | --- | --- |
| Loss or theft of a tester | Physical loss of the tester; no intended disclosure of personal data or an on-device tag database | Data minimization limits the stored data set. Hardware-bound FRAM protection and encrypted flash raise the effort required to reuse copied storage. The device is not a physical anti-theft system and an opened, modified unit is outside the supported configuration. |
| Read-out of a saved Wi-Fi configuration or backup | Exposure of a local Wi-Fi password | Passwords are not shown in the normal device interface or emitted as plain values in the standard export; credential-bearing exports and persistent parameters receive separate protection. SSIDs remain visible by design and must not be treated as secrets. |
| Discovery of the device access point | Discovery of the local service network name | The AP exists deliberately for local operation. Its visible SSID is not a credential; access remains subject to the configured local access protection. The device does not route traffic to the Internet or provide cloud access. |
| Copying an external FRAM image | Attempted cloning or recovery of configuration data on another unit | The provisioning binding checks the association with the ESP using a salted, private HMAC-based tag. This is a cloning-resistance measure, not a guarantee against all physical attacks. |
| Physical access to Flash, PSRAM or debug pads | Attempted extraction of firmware, transient buffers or secrets | Flash Encryption, disabled JTAG interfaces and internal-SRAM-only key handling reduce the practical attack surface. External-PSRAM encryption protects transport to the memory where enabled. Invasive laboratory attacks remain outside the intended protection boundary. |

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
