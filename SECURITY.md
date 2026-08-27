# 🛡️ Security Policy & Coordinated Vulnerability Disclosure (CVD)

The SVA-Fencing-Tester initiative is committed to maintaining the highest level of cybersecurity resilience, protecting intellectual property, and safeguarding pending patent workflows. 

If you discover a potential security vulnerability in our custom firmware, the local REST API, the encrypted `.ota` bundle format, or the embedded web application, we strongly encourage you to report it to us privately.

We actively monitor automated dependency alerts (GitHub Dependabot) for our webapp toolchain and apply ecosystem patches regularly before compilation.

**Please do not open a public GitHub Issue or public Discussion for security vulnerabilities.**

---

## 📋 Supported Versions & Hardware

We exclusively support and patch the active hardware revisions hosted in this portal. The underlying device firmware automatically detects the hardware revision at runtime.

| Hardware Revision | Firmware Branch | Security Status |
| :--- | :--- | :--- |
| **Revision 4 (Rev 4)** | `main` | ✔️ Actively Supported |
| **Revision 2 (Rev 2)** | `main` | ✔️ Actively Supported |

*Note: Since the project is in active development, there are currently no deprecated or unsupported production versions available.*

---

## ✉️ How to Report a Vulnerability

To ensure full confidentiality and protect the integrity of the device infrastructure, please follow our private reporting process:

1. **Private Contact:** Please use the secure, spam-protected contact details provided at the bottom of our official **[Regulatory Compliance & Impressum Document](./docs/compliance)**.
2. **What to Include:**
   * A detailed description of the vulnerability.
   * The active firmware version and build number (e.g., `0.7.0+71`).
   * A proof-of-concept (PoC) or step-by-step instructions to reproduce the behavior.
   * Potential impact scenarios on the device or local network.

---

## 🤝 Our Commitment (The Disclosure Process)

This project is fully operated on a voluntary, non-profit basis in our spare time. While cybersecurity is highly important to us and we aim to react promptly, please understand that voluntary capacity, professional workloads, or holidays may lead to temporary delays. 

We act according to the following best-effort timeline:

* **Acknowledgment:** We typically strive to acknowledge receipt of your report with a private response within **3 to 7 days**. 
* **Evaluation:** We will review and validate the bug to determine if it affects the encrypted core loop or the local network stack.
* **Coordinated Fix:** Verified vulnerabilities will be patched within our private development branch based on availability.
* **Deployment:** The fixed version will be compiled, packaged into the encrypted `.ota` custom bundle format, and pushed to the public `main` branch.
* **Public Advisory:** A security advisory detailing the fix will only be published (if necessary) after the patch is active and deployed to the fencing clubs to prevent exploits in the field.

Thank you for scanning responsibly, respecting our voluntary capacity, and helping us keep the fencing community secure!
