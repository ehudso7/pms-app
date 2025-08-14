# Security and Threat Model

## Introduction

This project is a cross‑platform mobile application built with Expo and React Native.  It is intended to store and manage sensitive user secrets (such as passwords or notes) entirely on the user’s device.  The app uses **`expo-secure-store`** to encrypt data at rest and **`expo-local-authentication`** to protect access with biometrics【205538492460329†L11-L19】.  The dependency on **`tweetnacl`** provides strong, modern cryptography for optional zero‑knowledge backups【205538492460329†L11-L19】.  There is no advertising or analytics; the architecture is local‑first with optional, user‑controlled encrypted backups.

## Assets to Protect

* **User Secrets** – the primary data stored in the app (passwords, secrets, secure notes).  Disclosure could lead to account compromise or privacy breaches.
* **Encryption Keys** – a randomly generated **32‑byte key** created on first run and stored in the platform keychain/keystore via Secure Store【205538492460329†L11-L19】.  This key is never persisted in SQLite and is used with `tweetnacl.secretbox` to encrypt each record before it is saved.  Leakage would permit decryption of the vault.
* **Biometric Data** – used to unlock the vault via the OS’s biometric API.  We never access raw biometric templates; however, improper usage could allow bypass.
* **Configuration & Metadata** – settings such as whether backups are enabled, backup location and version, and key derivation parameters.

## Adversaries and Motivations

1. **Opportunistic attackers** who steal a device and attempt to extract secrets from on‑device storage.
2. **Malware or trojan apps** on the same device trying to read app data from disk or intercept secrets from memory.
3. **Network adversaries** intercepting data during optional backup/sync operations.
4. **Supply‑chain attackers** compromising third‑party dependencies or injecting malicious updates.
5. **Privileged attackers** (e.g. root/jailbreak users or state‑level actors) who can bypass OS protections.

## Attack Surface

| Surface                    | Threats                                                                        | Mitigations |
|---------------------------|--------------------------------------------------------------------------------|-------------|
| **On‑Device Storage**     | Extraction of secrets from the device file system or memory; brute‑force of vault encryption. | A 32‑byte symmetric key is generated and stored in Secure Store (never written to SQLite)【205538492460329†L11-L19】.  Each record is **encrypted before it is persisted**: the record is serialised to JSON, encrypted using `tweetnacl.secretbox` with a random nonce, and the tuple `{nonce, ciphertext}` plus a date index is stored in SQLite.  The master passphrase is used only to derive a user‑authentication key via Argon2, which unlocks the symmetric key.  Rate‑limit passphrase attempts and lock the vault after repeated failures. |
| **Biometric Unlock**      | Bypass of biometric authentication, spoofing fingerprints/face.                | Use the OS biometric APIs via `expo-local-authentication`【205538492460329†L11-L19】 to present a biometric gate each time the app is opened.  In v1.1, offer a fallback passcode screen.  The biometric challenge is mandatory unless the user explicitly disables it. |
| **Export/Backup**         | Interception or compromise of backups in transit or at rest.                   | Provide a **zero‑knowledge export/import**: users can create an encrypted archive of their vault and store it anywhere.  In the MVP the archive is encrypted with the on‑device symmetric key.  In v1.1, introduce a **passphrase‑derived key** using Argon2id for backups; keep that passphrase out of device backups.  Always transmit backup files over TLS.  We never keep any backup keys or passphrases on our servers. |
| **Inter‑Process Communication** | Malicious apps reading clipboard or using accessibility services to capture secrets. | Avoid copying sensitive values to the clipboard by default.  Provide a “copy” button that clears the clipboard after a short timeout.  Detect and warn if the device is rooted/jailbroken or if accessibility debugging is enabled. |
| **User Interface**        | Shoulder‑surfing or screen capture of secrets.                                 | Obscure secret values by default (mask characters) and reveal them only upon explicit user action.  Detect screenshot events where possible and display a warning. |
| **Dependency/Supply Chain** | Vulnerabilities in Expo, React Native or crypto libraries.                   | Pin dependency versions and update them regularly.  Use dependable sources (NPM, GitHub) and monitor CVE announcements.  Run automated dependency scanners.  Because we include no advertising or analytics SDKs, the attack surface from embedded third‑party code is reduced. |
| **Development & Build**   | Insertion of malicious code during development or CI/CD.                       | Use signed commits and review pull requests.  Generate release builds in a reproducible environment.  Sign Android/iOS packages and verify integrity before publishing. |

## Out‑of‑Scope Threats

* **Compromised Operating Systems** – Rooted or jailbroken devices can bypass Secure Store protections.  We will detect and warn, but cannot guarantee security on compromised devices.
* **State‑level actors** – Attackers with the ability to compromise mobile platforms at the firmware or baseband level are beyond our threat model.
* **Physical Coercion** – An attacker forcing a user to unlock the vault cannot be technically mitigated.

## Secure Development Practices

1. **Least privilege:** Limit app permissions to only those required for its core functionality.  Do not request network or location access unless optional backup features are enabled.  Avoid embedding analytics or advertising SDKs – privacy‑preserving telemetry may be added later but is currently absent.
2. **Key management:** Generate a 32‑byte symmetric key on first run, store it in Secure Store and never write it to SQLite.  Use Argon2id to derive separate keys for user authentication and backup encryption.  Clear keys from memory when not in use.
3. **Encryption before persistence:** Serialise secrets to JSON and encrypt them with `tweetnacl.secretbox` using a random nonce before writing to the database.  Store only `{nonce, ciphertext}` plus a date index.
4. **Code review:** All changes are peer‑reviewed via pull requests.  Use static code analysis and linting to catch common issues.
5. **Dependency management:** Keep dependencies up‑to‑date.  Monitor vulnerabilities in Expo, React Native and crypto libraries.  Apply patches promptly.
6. **Testing:** Implement unit and integration tests for cryptographic functions, authentication flows, biometric gating and backup/restore functionality.  Perform regular penetration testing.
7. **Secret handling:** Never hardcode secrets or encryption keys.  Use environment variables for build‑time configuration (but exclude them via `.gitignore`【496697045487838†L0-L37】).  Do not log secrets or passphrases.

## Responsible Disclosure

We take security seriously and appreciate community efforts to identify vulnerabilities.  If you discover a security issue in this project, please report it privately by emailing **security@yourdomain.example**.  Do not submit sensitive information in public issues.  We will triage reports promptly and work with you to remediate verified vulnerabilities.
