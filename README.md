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
### L1: Why do we need SPICE simulations?              **********
**CMOS Inverter Basics**
- Focusing on the fundamentals of circuit design and SPICE simulations for analyzing and validating electronic circuits. Starting with the Complementary Metal Oxide Semiconductor(CMOS) structure, which is the foundation of modern integrated circuit technology. CMOS is designed to enable low-power, high-efficiency operation. Understanding this structure is essential for practical circuit implementation and simulation.
<img width="345" height="364" alt="Screenshot 2026-02-16 182139" src="https://github.com/user-attachments/assets/b83a795f-aa0f-4db3-931e-31dac6cad96b" />

**Delay Concepts**:
An essential part is spice simulations, which help evaluate timing behavior, switching response, and propagation delay under different design and loading conditions. This allows precise performance estimation before practical implementation.
- Delay depends on input slew and output load
- Combined input slew & output load -> delay table value
- Each transistor (PMOS & NMOS) is unique, with different delay values
- Buffer sizing (W & L) determines current and delay behavior. Buffer differentiation is part of circuit design.
- Delay Table

<img width="504" height="415" alt="image" src="https://github.com/user-attachments/assets/6a8fdc98-cbc9-4e4a-9fbf-8040b66298ba" />

The delay table shows the delay for combinations of:
- Input slew values
- Output load capacitances

=> Width (W) and length (L) decide the Drive current (I) and the resulting timing curve

### L2:  Introduction to basic elements in Circuit design – NMOS              **********
- A transistor is a semiconductor device that controls current flow through an electric field and functions mainly as a switch or an amplifier. The MOS transistor is the core device used in CMOS circuits. A MOS transistor can be either n-type (NMOS) or p-type (PMOS) depending on the dominant charge carriers and doping.
<img width="405" height="405" alt="Screenshot 2026-02-16 173838" src="https://github.com/user-attachments/assets/f7df0803-1f4e-454e-acd4-dacbe8745648" />

A transistor is a 4-terminal device. With its terminals as Gate, Source, Drain, and Body/Substrate. 
Taking an NMOS device, it has 
- p-type substrate
- n+ Source and Drain
- Isolation region (generally SiO2)
- Gate Oxide
- Poly Si or Metal Gate

In logic schematics, the body is often tied to the supply, so only three terminals are shown.

### L3: Strong inversion and threshold voltage **********
**Threshold Voltage (V<sub>t</sub>)**
- It is the minimum gate-to-source voltage required to create a strong inversion layer in a MOS transistor so that a conductive channel forms between source and drain. Below V<sub>t</sub>, the transistor is effectively “off”.

- A lower threshold means easier turn-on; a higher threshold means less off-state leakage.

- In digital circuits, V<sub>t</sub> is a key design parameter for switching and leakage behavior.
<img width="505" height="450" alt="Screenshot 2026-02-16 184952" src="https://github.com/user-attachments/assets/a51d8632-cd7c-42b9-b8a2-2c3d4ae6e9d4" />


**Strong Inversion**
- When V<sub>GS</sub> (gate-to-source voltage) is above the threshold voltage enough that the semiconductor surface beneath the gate behaves like the opposite type (e.g., n-type for an n-MOSFET).

- In this region, the channel has many charge carriers and current flows mainly by drift.

- Strong inversion marks the MOSFET being fully “on,” with a well-formed conducting channel.

### L4: Threshold voltage with positive substrate potential  **********
Initially, there was no source voltage, so the threshold voltage was unaffected by any external factors.
<img width="405" height="405" alt="Screenshot 2026-02-16 185412" src="https://github.com/user-attachments/assets/8b079781-cf30-4ee0-8116-57a4e2cdc83a" />

Now, as positive Vsb results in a flow of electrons from the source to the body, the inversion voltage increases. As inversion near the source is hindered by Vsb.
It adds the additional terms to the threshold voltage, as shown in the image below.
<img width="2846" height="1492" alt="Screenshot 2026-02-16 190408" src="https://github.com/user-attachments/assets/3407d959-fdfe-464e-852b-5db5855d6433" />

- We are given values of constants and parameters. Use these values directly in SPICE simulations. After running simulations and calculations, the output is obtained as Vt. Under certain applied bias conditions between Vsb, the threshold voltage is evaluated.
<img width="305" height="305" alt="7477c2e5-41fc-408b-ad80-48b965c6b069" src="https://github.com/user-attachments/assets/1cdb4330-9e35-479a-a80d-0a2da331ab87" />

- These are some simple Models that we can simulate on SPICE.

### NMOS resistive region and saturation region of operation
### L1: Resistive region of operation with small drain-source voltage  **********
- In this region, V<sub>GS</sub> > V<sub>t</sub> with the small applied drain voltage V<sub>DS</sub>. It is valid for certain V<sub>DS</sub> only, which will be discussed later.
<img width="800" height="480" alt="Screenshot 2026-02-16 194916" src="https://github.com/user-attachments/assets/e0743351-9d47-4849-bb88-4f98ca40460c" />

- It is observed that the voltage across the channel is no longer constant after applying V<sub>DS</sub>. There is a linear voltage gradient.
- It also impacts the channel length. The Effective channel length is therefore lower than the original channel length.
<img width="2880" height="1800" alt="Screenshot 2026-02-16 195420" src="https://github.com/user-attachments/assets/96fec749-9396-45b5-950e-6925e490c30f" />

### L2: Drift current theory  **********
Now that we know there is a voltage gradient, let's relate it to the applied V<sub>ds</sub>.
  <img width="2818" height="1308" alt="image" src="https://github.com/user-attachments/assets/0ea38acc-ce44-4e75-93a5-3e116a85c07e" />
**Drift Current**: The current induced by the potential difference between the source and the drain.
**Diffusion Current**: The current induced due to the concentration gradient.

### L3: Drain current model for linear region of operation  **********
Some of the calculations are in the image below.
![20260217_155236 jpg](https://github.com/user-attachments/assets/8ed89a7c-616d-4285-ac67-fba23e87048e)

<img width="2734" height="1567" alt="image" src="https://github.com/user-attachments/assets/4f30f095-bdd8-4d54-9d16-5b61279bd478" />

<img width="2880" height="1800" alt="Screenshot 2026-02-16 212921" src="https://github.com/user-attachments/assets/88f12cd0-723f-4686-976e-b0840bdf7256" />

### L4: SPICE conclusion to resistive operation  **********
- In the SPICE simulations, now we will command it accordingly for swiping the values as shown in the image.
- A value till where the current is increasing is being checked, and the curve of current w.r.t Vds is being analysed using SPICE simulations.
<img width="2647" height="1484" alt="image" src="https://github.com/user-attachments/assets/89ee9a1e-5811-49ef-be03-68ec20b0afc3" />

### L5: Pinch-off region condition  **********
![WhatsApp Image 2026-02-17 at 4 20 14 PM](https://github.com/user-attachments/assets/f351873d-eb36-46db-9c46-ec0a9f53714c)

<img width="2880" height="1800" alt="Screenshot 2026-02-16 214707" src="https://github.com/user-attachments/assets/a44fd5e3-2a13-4218-8ba2-07904279247f" />
<img width="2880" height="1800" alt="Screenshot 2026-02-16 220029" src="https://github.com/user-attachments/assets/5a210071-880a-4170-88a6-d6cf053f316a" />

### L6: Drain current model for saturation region of operation  **********
![WhatsApp Image 2026-02-17 at 4 23 02 PM](https://github.com/user-attachments/assets/b2fa1afa-9f1e-4c72-a9e3-40ea89fe2e1d)

### Introduction to SPICE
### L1: Basic SPICE setup  **********
![WhatsApp Image 2026-02-17 at 4 32 12 PM](https://github.com/user-attachments/assets/6e2cb116-cd94-4d7b-bd15-656d23016795)
<img width="2880" height="1800" alt="Screenshot 2026-02-17 163237" src="https://github.com/user-attachments/assets/c5b3903d-8648-489f-beba-fee955665ef5" />

### L2: Circuit description in SPICE syntax  **********
- Representation of the MOSFET in the form of nodes and writing the code.
<img width="2028" height="591" alt="image" src="https://github.com/user-attachments/assets/23fe03e6-4394-4cc0-b85a-5497db610352" />

- Technology File syntax for the simulations
![WhatsApp Image 2026-02-17 at 4 47 40 PM](https://github.com/user-attachments/assets/30a7e46e-aa5e-4b43-ba76-18909e184920)

### L3: Define technology parameters  **********
- We have model parameters that we keep in the simulation file. Those depend on the device's technology node.
- The file from which model parameters are taken is generally a .mod file. Whether NMOS or PMOS, those parameters are extracted from the .mod file during simulation.
- Here, SPICE simulations will help us sweep the voltage levels in the required range using some commands.

### L4: First SPICE simulation  **********
- As Vt is around 0.55V, so for 0.2V and 0.4V there is no curve observed.
![WhatsApp Image 2026-02-17 at 10 41 58 PM](https://github.com/user-attachments/assets/11843c0f-e384-4f44-8c8b-99740f523a55)

### SPICE simulation for lower nodes and velocity saturation effect
### L1: SPICE simulation for lower nodes  **********
- We can see the behaviour of the transistor in the linear regime, which is completely different from the saturation regime due to the pinch-off phenomenon. Generally, it occurs at higher V<sub>ds</sub>.

- We also see that in the saturation region, it seems constant, but it is not. It varies as a function of λ*V<sub>ds</sub>. 
<img width="2177" height="1339" alt="image" src="https://github.com/user-attachments/assets/072273b8-4bb5-4246-8fc3-2d1f68e0fdf3" />

### L2: Drain current vs gate voltage for long and short channel devices  **********
- Simulation for W = 0.375u, L = 0.25u, W/L = 1.5 and W=1.8u, L=1.2u, W/L = 1.5

| W=0.375u and L=0.25u | W=1.8u and L=1.2u |
|---|---|
|<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/79a88d00-7b31-499f-912e-1993c1161173" /> | <img width="400" height="400" alt="Screenshot 2026-02-18 151631" src="https://github.com/user-attachments/assets/084e9800-e549-46a2-a03e-20544fbdc411" /> |
|<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/b933fcd1-c197-4ebc-b73f-50896ce5646d" /> |<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/959a0ec5-0a0a-4635-9e2f-4215fa929af8" /> | 

- We can see the Quadratic dependence of Vds in a long-channel MOSFET and the Linear dependence of Vds in a short-channel MOSFET.
- Any device below 0.25u length is considered a short-channel MOSFET.
- We can also see the Velocity saturation.

### L3: Velocity saturation at lower and higher electric fields  **********
<img width="2200" height="1000" alt="image" src="https://github.com/user-attachments/assets/bad42649-3531-4826-89d6-a9e28326e1ff" />
<img width="2138" height="685" alt="image" src="https://github.com/user-attachments/assets/94446fa2-5d67-4481-bd96-670057b1e63d" />
<img width="1080" height="1529" alt="image" src="https://github.com/user-attachments/assets/5abb1ebf-17e9-44ca-8b6d-5eb7a0d97743" />

### L4: Velocity saturation drain current model  **********
- We know that Vdsat is a technology parameter that we will get from Fab.

- Substituting Vgs - Vt in the simpler equation. Means Vgt is Minimum and behaves in a saturation regime.
<img width="300" height="60" alt="image" src="https://github.com/user-attachments/assets/ecc05bff-ccb7-450f-9c74-8e2004e8fbdc" />

- When Vds is Minimum, it behaves in a resistive regime.
<img width="300" height="55" alt="image" src="https://github.com/user-attachments/assets/04057e84-8640-4380-b7d5-48b0b246f961" />

- We can have Vdsat minimum for lower nodes devices, so that is only applicable to devices length <250nm.
<img width="1000" height="120" alt="image" src="https://github.com/user-attachments/assets/103afe49-6210-42d7-98a0-4fb1e983cee2" />

### L5: Labs Sky130 Id-Vgs  **********
- Id vs Vdd
<img width="400" height="400" alt="image" src="https://github.com/user-attachments/assets/6a24c400-e613-467f-b8af-aee0500276f7" />
<img width="1206" height="757" alt="image" src="https://github.com/user-attachments/assets/dfe506ed-786e-46d2-91e6-6df8d67ab369" />

### L6: Labs Sky130 Id-Vt
- Id vs Vgs
<img width="500" height="460" alt="image" src="https://github.com/user-attachments/assets/4e38e2d0-9849-4a07-8d02-f1ae0d8dc8d4" />
Vthreshold = 0.783 V

