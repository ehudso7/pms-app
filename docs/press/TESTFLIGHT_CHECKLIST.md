---
title: TestFlight & Closed Testing Checklist
---

# TestFlight / Closed Testing Checklist

This checklist helps testers exercise the core functionality of pms‑app before public release.  Use it for TestFlight (iOS) or Google Play closed testing.

## Pre‑Test Setup

1. **Install the app:** Use the TestFlight or Play Store invite link.  Ensure that you remove any previous builds to avoid conflicts.
2. **Device details:** Record your device model and OS version.  Test on a variety of devices (iOS and Android, old and new) to catch platform‑specific issues.
3. **Network conditions:** Perform tests both online and offline to verify behaviour.  Optional backups require an internet connection; core vault functions should work offline.

## Functional Tests

1. **Onboarding and Setup**
   - Launch the app.  Verify welcome screen messaging (local‑first, zero‑knowledge).  
   - Create a master passphrase and confirm passphrase strength indicator works.  
   - Enable biometric unlock and verify the OS prompts for Face ID/Touch ID/fingerprint.

2. **Vault Operations**
   - Add a new password entry with fields (title, username, password, notes).  Save and confirm it appears in the list.  
   - Edit the entry (change password or notes) and confirm changes persist.  
   - Delete the entry and verify it is removed from the vault.  Undo/restore if the feature exists.

3. **Secure Display & Copy**
   - Open an entry and ensure the password is masked by default.  
   - Tap to reveal the password; verify it hides again after a set timeout or on navigation away.  
   - Copy the password and ensure the clipboard clears after a short interval.

4. **Search and Organisation**
   - Add multiple entries and use the search function to filter by title or username.  
   - Verify sorting or tagging features if present.

5. **Biometric Unlock & Lockout**
   - Lock the app and reopen it.  Ensure biometric prompt appears and unlocks the vault.  
   - Simulate failed biometric attempts; verify fallback to passphrase and rate‑limiting of attempts.  
   - Change biometric preference in settings and ensure it takes effect.

6. **Backups & Restore (Optional Feature)**
   - Enable encrypted backups.  Choose a cloud provider and create a backup.  
   - Verify that an encrypted backup file appears in your storage provider.  
   - Delete local data (e.g. uninstall/reinstall the app) and attempt to restore from the backup using your master passphrase.  
   - Ensure that restoration works only with the correct passphrase and fails gracefully with an incorrect one.

7. **Settings and Preferences**
   - Toggle dark/light mode and verify UI updates accordingly.  
   - Change language/locale (if supported) and ensure text is translated correctly.  
   - Review the privacy policy and data safety statements accessible from within the app.

8. **Error Handling & Edge Cases**
   - Attempt to create a very long entry or use unusual characters; ensure there are no crashes.  
   - Force‑quit the app during a backup; reopen and verify consistency.  
   - Test on a device with low storage and ensure the app handles "disk full" gracefully.

## Non‑Functional Tests

* **Performance:** Monitor app responsiveness when loading a vault with hundreds of entries.  Transitions should remain smooth.
* **Battery Usage:** Use the app over an extended period and note any abnormal battery drain.
* **Accessibility:** Verify that screen readers (VoiceOver/TalkBack) can navigate the app.  Ensure that buttons and inputs have descriptive labels.

## Reporting

Please report issues through the TestFlight “Send Feedback” feature or via email to **testing@yourdomain.example**.  Include:

* A clear description of the issue and steps to reproduce.
* Screenshots or screen recordings if possible (exclude real secrets; use demo data).
* Your device model, OS version and app build number.

Thank you for helping to ensure pms‑app is secure, stable and user‑friendly before its official launch!
