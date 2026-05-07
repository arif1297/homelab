# Issue: Ubuntu Installer Not Detecting SSD

## Issues Observed
- Ubuntu Server installer did not recognise SSD as a bootable storage device.
- Installation could not continue due to no available storage device

## Investigation
Checked BIOS settings to confirm whether the SSD was being detected by the system.

## Resolution
- Entered the system BIOS
- Changed storage controller mode from RAID / RST to AHCI
- Saved changes and rebooted the system
- Restarted the Ubuntu Server installer

## Outcome
- SSD appeared correctly in the installer
- Ubuntu Server installed successfully on the SSD
