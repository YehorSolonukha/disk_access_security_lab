# Experiment 2: Performing unautorized CRUD operations on real unencrypted disk

## Objectives:
- Demonstrate how Full Disk Encryption (FDE) neutralizes the CRUD attack by ensuring that data at rest is unreadable and can't be modified even if the drive is physically moved to an attacker's machine
- Highlight the difference between mitigating attacks that target Confidentiality/Integrity vs Availability

## Results
- Outcome: Full administrative access to the host's files was achieved without ever entering a Windows password.
- Conclusion: OS-level login credentials provide zero protection against an attacker with physical access to an unencrypted drive.The hardware will trust any OS it is told to boot unless further measures are taken.

## Environment
- ref. experiment2

## Execution Steps:
Target Setup
- Open File Explorer, right-click the C: drive, and select Turn on BitLocker.
- Wait until encryption process is completed (you can check status with ```manage-bde -status``` if needed)

Attacker Setup
- ref. experiment2

Gaining Access 
- ref. experiment2

! CRUD Operations !
1. Identify the Target Partition (should have no mounpoints, might have label SYSTEM next to it, usually ntfs type):
```
lsblk -f
```
2. Mount the Partition:
```
sudo mount /dev/<target_partition> /mnt
```
3. Navigate to directory and perform CRUD operations (CRUD ops. not included):
```
cd /mnt
```

Verification
1. Unmount the drive: 
```
sudo umount /mnt
```
2. Shut down the Live OS and remove the USB.
3. Boot back into Windows 11 to observe that files have been moved, modified, or deleted...