# Objective
Present most effective techniques integrated into Defense-In-Depth strategy, including: Full Disk Encryption (FDE), Firmware Lockdowns (BIOS password, disable USB boots, disable entering Bootloader), and Secure Boot preventing unauthorized data exfiltration and system tampering.

## Mitigation Layer 1
Full Disk Encryption (FDE)

### Defensive Goal
Ensure that even if an attacker successfully mounts our root filesystem, the data remains inaccessible (note: but can be destroyed/ encrypted).

### Technique
Install OS using LUKS with LVM or use other alternatives (BitLocker for Windows, Filevault for Mac)

---
## Mitigation Layer 2
Disable Booting from USB

### Defensive Goal
Prevents an attacker from booting a Live USB

### Technique
1. Set a BIOS password in BIOS>Security settings !C
2. Disable Booting from USB !C AND Disable F12 Boot Menu
3. Set the Internal Hard Drive as the sole boot device

---

## Mitigation Layer 3
System Integrity (Secure Boot)

### Defensive Goal
Prevents attacks where an attacker modifies the unencrypted bootloader or kernel to plant malware/keyloggers.

### Technique
Enable EFI and Secure Boot in BIOS Settings to examine each component in Boot Sequence (BIOS/UEFI, Bootloader, Kernel) for its authenticity