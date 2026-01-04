## Objectives:
- confirm, in practice, the following hypothesis: if a file-system is not encrypted it can be accessed and its data copied by an independent OS without logging into the original OS
- prepare for further experiments by conducting fundamental tests

## Experimental Model (What we're working with):
VirtualBox (type 2 hypervisor for our VMs)

VM 'Target' (Linux Mint) + virtual disk 'Target'
VM 'Threat Actor'(Linux Mint) + 2 virtual disks ('Target' and 'Threat Actor')

## Threat Model (What we are trying to do):
Target:
1. full OS installation
2. creates test files 

Threat Actor:
1. full OS installation
2. mounts VD of a 'Target' that contains data onto a mount point inside his OS in read-only mode 
3. copies all data into another DIR
4. disattaches VD 'Target' and checks whether data persists

## Results:

## Concrete Steps to Reproduce:
Target:
- create a VM 'Target'; provide username, password and ISO
- start up the VM, provide ISO
- install Linux Mint (repeat username and password, do not enable 'encrypt home directory'), wait for full installation
- click 'Restart Now' after installation succeeded
- log in
- open terminal
- run the following command to create test files (replace 'your_username' with username you provided when installing OS): cd /home/your_username && mkdir TEST_DIR && cd TEST_DIR && echo "TEST_TEXT" > TEST_FILE
- power off the VM


Threat Actor:
- create a VM 'Threat Actor'; provide ISO and username + password (different from 'Target' although it shouldn't matter)
- start up the VM only when 'Target' VM is powered off and OS installation succeeded, provide ISO
- install Linux Mint (repeat username and password)
- after installation succeeded -> restart VM -> power off VM after restart
- open Settings for that VM -> Storage -> under 'Controller SATA' choose 'Adds Hard Disk' -> choose a disk that corresponds with the name of a 'Target' VM -> click 'Ok'
- start up the VM
- open terminal
- run the following command to see what disks are available to our system and what file systems exist on those disks: lsblk -f
- identify a partition of a disk which has ext4 file-system and no mount-point (the name of that partition will be something like 'sdb3')
- run the following command to mount that fs onto our system in read-only mode to make it visible (replace 'your_disk' with the name of identified disk from previous step, provide password when prompted): sudo mount -r /dev/your_disk /mnt
- run the following command to obtain username of our target: ls /mnt/home
- run the following command to copy target data into our home directory (replace 'your_target_username' with a username obtained from previous step): sudo cp -r /mnt/home/your_target_username /home/ALL_TARGET_DATA
- run the following command to verify that data has been copied: sudo ls /home/ALL_TARGET_DATA
- power off VM
- go to Settings -> Storage -> identify 'Target' VD and remove it -> click 'Ok'
- start up the VM
- run the following command to verify that the data persists: sudo ls /home/ALL_TARGET_DATA