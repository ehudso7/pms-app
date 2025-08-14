---
title: Privacy Policy
---

# Privacy Policy

_Effective Date: August 13, 2025_

This privacy policy explains how the **pms‑app** mobile application (“we”, “us”, or “our”) handles your information.  Our philosophy is **local‑first**: your secrets stay on your device.  The app does **not** include ads or third‑party trackers, and there is no analytics SDK baked in.  On first launch we generate a random **32‑byte encryption key** and store it securely in the OS keychain/keystore via Secure Store; this key never leaves your device and is not persisted in SQLite.

## Information We Collect

We intentionally collect **very little** information:

| Category                   | What we collect                     | Purpose                     |
|----------------------------|-------------------------------------|-----------------------------|
| **User secrets**           | Passwords, secure notes and other data you voluntarily enter into the app. | Stored locally on your device and encrypted using secure cryptography.  Never sent to us. |
| **App settings**           | Preferences such as biometric unlock, backup configuration and theme selection. | Used to customise your experience.  Stored locally and encrypted. |
| **Error logs (optional)**  | Non‑personal crash reports (e.g. stack traces, OS version, device model). | If you opt in to send crash reports, we use this information to diagnose and fix bugs.  Crash logs do **not** include any of your secrets. |
| **Encryption metadata**    | A random nonce and date index associated with each encrypted record. | Stored alongside the ciphertext in the app’s SQLite database.  These values do **not** reveal your secrets and exist solely to support decryption and sorting. |

We **do not** collect your name, email address, contact list, device identifiers or location.  We do not have any server that stores user databases; there are no ads and no usage analytics.

## How We Use Information

* **To provide the core functionality.**  The app uses your secrets and settings to display your vault and allow you to search, edit and organise entries.
* **To secure your data.**  We generate a 32‑byte symmetric key and store it in Secure Store (never in SQLite).  Each entry is serialised to JSON and encrypted with `tweetnacl.secretbox` using a random nonce **before** it is written to the database.  Optionally, if you enable cloud backups, we wrap the vault in another layer of encryption with a key derived from your master passphrase (zero‑knowledge) before leaving your device.  We do not have access to the backup encryption key.
* **To improve reliability.**  If you opt in, crash reports help us diagnose issues.  Crash reports never contain your secret data.

## Data Sharing and Third Parties

We do **not** sell, rent or share any of your data.  We do not partner with advertisers.  There is no analytics SDK baked in today (we may add privacy‑preserving telemetry in a future version, but it will be opt‑in).  The only third‑party services used are:

* **Operating system providers (Apple/Google).**  They provide keychain/keystore functionality used by Expo Secure Store.  They do not have access to your vault contents.
* **Optional backup service.**  You may choose a cloud provider (e.g. iCloud Drive or Google Drive) to store an encrypted backup.  The provider stores an encrypted blob; without your backup key it is unintelligible.

## Data Retention

All user secrets and settings remain on your device.  When you delete an entry, it is immediately removed from the vault.  If you uninstall the app, all local data is erased.  If you created a backup, you must delete it manually from your chosen storage provider.

## Your Choices

* **Use of biometrics.**  You can choose to enable biometric unlock via Face ID/Touch ID (iOS) or fingerprint (Android).  Disabling biometrics will require your master passphrase to unlock the app.
* **Backups.**  You may opt in or out of encrypted backups.  If enabled, you choose where the backup file is stored.  Backups can be deleted at any time.
* **Crash reporting.**  You control whether crash logs are sent.  Opting out will not affect app functionality.

## Security Measures

We protect your data with industry‑standard cryptography.  Secrets are encrypted on device using keys derived from your master passphrase.  Biometric unlock relies on the secure enclave/keystore provided by the OS.  We use `tweetnacl` to implement modern cryptographic primitives【205538492460329†L11-L19】.  Although we strive to safeguard your data, no system can guarantee absolute security.  Use a strong, unique master passphrase and keep your device’s OS up‑to‑date.

## Children’s Privacy

This app is not designed for children under 13.  We do not knowingly collect personal information from children.  If we learn that a child has provided us with personal information, we will delete it immediately.

## Changes to This Policy

We may update this Privacy Policy from time to time.  When we do, we will notify you via an in‑app message or by updating the “Effective Date” at the top of this document.  Continued use of the app after updates means you accept the revised policy.

## Contact Us

If you have any questions or concerns about this privacy policy or our data practices, please contact us at **privacy@yourdomain.example**.
