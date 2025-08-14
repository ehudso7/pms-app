---
title: Data Safety Statement
---

# Google Play Data Safety

The Google Play Data Safety section below summarises how **pms‑app** handles user data.  It is intended to accompany our full [Privacy Policy](./PRIVACY_POLICY.md).

## Data Collection and Sharing

| Data category         | Collected? | Shared with third parties? | Purpose |
|-----------------------|-----------:|---------------------------:|---------|
| Personal Information  | No         | No                         | N/A     |
| Contacts              | No         | No                         | N/A     |
| Location              | No         | No                         | N/A     |
| Financial Info        | No         | No                         | N/A     |
| Health Info           | No         | No                         | N/A     |
| Sensitive data        | No         | No                         | N/A     |
| Crash logs (optional) | Yes        | No                         | Diagnostics (if user opts in). |
| App interactions      | No         | No                         | N/A     |
| Device ID/other IDs   | No         | No                         | N/A     |

*We do **not** collect any personal identifiers or behavioural data.  Optional crash logs, if enabled, are anonymised and used solely to fix bugs.*

## Data Processing and Storage

* **On‑device storage:** A random 32‑byte symmetric key is generated on first run and stored in the OS keychain/keystore via `expo-secure-store`.  Each record is serialised to JSON and encrypted with this key using `tweetnacl.secretbox` and a random nonce **before** being written to SQLite.  Data never leaves the device unless the user enables backups.
* **Backups:** If the user chooses to back up their vault, the data is encrypted with a key derived from their master passphrase (zero‑knowledge).  The encrypted backup file is uploaded to the user’s chosen storage provider (e.g. iCloud Drive or Google Drive).  We do not have access to encryption keys or backup contents.
* **Security:** Data in transit (e.g. during backup upload) is transmitted over HTTPS.  Encryption keys are never stored on our servers.  Biometric unlock relies on secure hardware.

## Data Retention and Deletion

* Data is retained only on the user’s device.  When an entry is deleted in the app, the encrypted record is removed immediately.
* Backups are retained only as long as the user keeps the encrypted backup file in their storage provider.  Users can delete backups at any time.
* Uninstalling the app removes all local data.

## Data Safety Summary for Play Store

* We **collect**: only optional anonymised crash logs (no personal identifiers).
* We **share**: nothing with third parties.  There are no advertising or analytics SDKs embedded in the app.
* We **process**: sensitive user secrets entirely on device, encrypting each record before persistence and using a device‑stored key.
* We **retain**: data only on the user’s device; backups remain fully in the user’s control.
* We **protect**: data with a Secure Store–managed 32‑byte key, `tweetnacl` encryption and biometric authentication.

# Apple App Store Privacy

For the Apple App Store’s privacy “nutrition label”, pms‑app falls under the following categories:

| Category                                    | Collected? | Linked to user? | Used for tracking? |
|---------------------------------------------|-----------:|---------------:|-------------------:|
| Contact Info (name, email, phone)           | No         | N/A            | N/A               |
| Health & Fitness                            | No         | N/A            | N/A               |
| Financial Info                              | No         | N/A            | N/A               |
| Location                                    | No         | N/A            | N/A               |
| Sensitive Info (passwords, notes)           | Yes        | No             | No                |
| User Content (files, docs)                  | No         | N/A            | N/A               |
| Browsing History                            | No         | N/A            | N/A               |
| Identifiers (device ID, user ID)            | No         | N/A            | N/A               |
| Diagnostics (crash logs)                    | Optional   | No             | No                |
| Usage Data (analytics)                      | No         | N/A            | N/A               |

*Sensitive information (passwords, secure notes) is stored locally on device and encrypted.  It is not linked to you because we do not collect any identifiers.  No data is used for tracking.*

## Contact

Questions about data safety can be sent to **privacy@yourdomain.example**.
