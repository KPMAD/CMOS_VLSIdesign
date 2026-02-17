# CMOS_VLSIdesign
## A) Virtual Machine Setup for SPICE simulations
1. Click on the link below for the Installation of VM
   - Windows: https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6a-172322-Win.exe
   - MacOS/Intel hosts: https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6-172322-OSX.dmg
   - MacOS/Apple Silicon hosts: https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6-172322-macOSArm64.dmg
   - Linux: https://www.virtualbox.org/wiki/Linux_Downloads
   - Solaris hosts: https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6-172322-SunOS.tar.gz
   - Solaris 11 IPS hosts: https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6-172322-Solaris.p5p
    
2. Launch VirtualBox and click on the "New" button to create a new virtual machine
3. Name the VM and select the VM folder. Select **OS** as **Linux** followed by **OS Distribution** as **Ubuntu**.
4. Select the **OS Version** compatible with the VDI file. Here I have chosen **Ubuntu 18.04 LTS (Bionic Beaver) (64-bit)**
5. **Specify Virtual Hardware** with "Base Memory - 4096MB" and "Number of CPUs - 4".
6. **Specify Virtual Hard Disk** by tick on *Use an Existing Virtual Hard Disk File*. Attach the VDI File by clicking on the yellow Folder button.
7. Click **Finish**
8. Now select the VM and click on **Start**.

## B) NgspiceSky130 - Day 1 - Basics of NMOS Drain current (Id) vs Drain-to-source Voltage (Vds)
### Introduction to Circuit Design and SPICE simulations
### L1: Why do we need SPICE simulations?
**CMOS Inverter Basics**
- CMOS inverter circuit with PMOS and NMOS

**Delay Concepts**
- Delay depends on input slew and output load
- Combined input slew & output load -> delay table value
- Each transistor (PMOS & NMOS) is unique, with different delay values
- Buffer sizing (W & L) determines current and delay behavior. Buffer differentiation is part of circuit design.
- Delay Table

The delay table shows the delay for combinations of:
- Input slew values
- Output load capacitances

=> Width (W) and length (L) decide the Drive current (I) and the resulting timing curve
