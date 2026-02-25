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

## B) Day 1 - Basics of NMOS Drain current (Id) vs Drain-to-source Voltage (Vds)
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

## C) Day 2 - Velocity saturation and basics of CMOS inverter VTC

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

### CMOS voltage transfer characteristics (VTC)
### L1: MOSFET as a switch
- By regulating the gate voltage, a MOSFET employed as a switch can function in two primary states: OFF (open switch) and ON (closed switch). To regulate current flow, we drive it fully ON or fully OFF rather than using it as an amplifier.
<img width="2878" height="1710" alt="image" src="https://github.com/user-attachments/assets/eda18bd7-6db4-4e5c-922a-bece85c8ba20" />

### L2: Introduction to standard MOS voltage current parameters
- CMOS in which the output node and the load capacitor C L are controlled by complementary switches made of PMOS and NMOS.

When Vin = Vdd (input HIGH), the NMOS and PMOS behave like complementary switches, as shown in the middle figure. For the NMOS, the gate-to-source voltage is Vdsn = Vin - Vss = Vdd​, which is greater than the threshold voltage, so the NMOS turns ON and behaves like a closed switch with a small equivalent resistance Rn. A drain–source current Idsn flows through the NMOS to ground, providing a discharge path for the output node. At the same time, for the PMOS, the gate and source are both near Vdd, so VGSP≈0, which is below its threshold magnitude, turning the PMOS OFF and making it act like an open switch. Therefore, there is no path from Vdd to the output. As a result, the load capacitor CL discharges through the NMOS, and Vout becomes 0.

<img width="2663" height="1356" alt="image" src="https://github.com/user-attachments/assets/e77e7765-c7d9-4180-956b-e1e1fdb80178" />

### L3: PMOS/NMOS drain current v/s drain voltage
- Because the source is grounded in NMOS, the analysis is straightforward: the input voltage is directly equal to the gate-to-source voltage, and the output voltage is equal to the drain-to-source voltage.So, VGSN​ = Vin

- In PMOS, the drain current flows in the opposite direction from NMOS. In relation to NMOS current, the PMOS drain current appears negative because the selected reference direction is negative.
<img width="350" height="350" alt="image" src="https://github.com/user-attachments/assets/eb3a1413-95dd-4f70-a2e1-6747f9ee8517" />

- The transistor switches on and the current rises when the gate voltage is beyond the threshold. The drain current increases further as the gate voltage is raised. Both linear (triode) and saturation areas are seen in the characteristic curve.
<img width="350" height="350" alt="image" src="https://github.com/user-attachments/assets/1775a901-7829-4b70-bae4-6a3b5bf2696b" />

- PMOS has the same behavior, however, with the opposite voltage polarity. Because the voltage and current polarities are inverted, the current-voltage curves appear in the negative quadrant.
<img width="350" height="390" alt="image" src="https://github.com/user-attachments/assets/55c4b633-b0cc-4884-82e9-0234b682172a" />

### L4: Step1 – Convert PMOS gate-source-voltage to Vin
- Only the input voltage (Vin) and output voltage (Vout) matter in digital circuit design. Internal node voltages, such as drain-to-source or gate-to-source voltages, are removed from the final modeling as they are intermediate variables.

Assuming that our device is a Long Channel.
- Vin​ = VGSP​ + VDD
- IdsP = -IdsN
- VdsP = Vout - Vdd​
<img width="400" height="325" alt="image" src="https://github.com/user-attachments/assets/bd150687-d26b-4850-930c-241a97420d24" />

<img width="700" height="350" alt="image" src="https://github.com/user-attachments/assets/255909c5-5712-44ba-bc1c-de4e610ce6ff" />

- The curves are adjusted to match the actual input voltage levels rather than being plotted against internal voltages. As a result, the model helps in the study of digital circuits and the determination of Voltage Transfer Characteristics (VTC), which subsequently helps in delay analysis.

### L5: Step2 & Step3 – Convert PMOS and NMOS drain-source-voltage to vout
- Steps 2 and 3 for PMOS
Vgsp is converted into Vin. Our approach is to convert Vdsp and the current into Vout and the current, respectively.
- Equation is VdsP = Vout - VDD, so by adding VDD, we get the Vout.
<img width="900" height="275" alt="image" src="https://github.com/user-attachments/assets/b35e5984-d3fb-47e5-8543-4c65212a8194" />

- By the graph, we can conclude that when our Vout = 0, it means our Capacitor is completely discharged, and when Vout = Max, which here is 2V, it means the Capacitor is completely charged, and the charging current is zero.

Equation for the relation of internal voltage and current in NMOS
- VgsN = Vin - Vss = Vin
- VdsN = Vout

- NMOS load curve creation
<img width="900" height="430" alt="image" src="https://github.com/user-attachments/assets/3d6dcf6d-9512-4773-b651-3642723611a3" />

- Load Curves of Both PMOS and NMOS in CMOS
<img width="900" height="295" alt="image" src="https://github.com/user-attachments/assets/1e7fc827-0af2-48b1-a7ad-cef66c6e7672" />

### L6: Step 4 – Merge PMOS – NMOS load curves and plot VTC
- Superimpose the Load curve of NMOS and PMOS to get the relation between Vin and Vout of the CMOS device.
<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/bb87eb54-4670-45bb-97e9-b8528d1488e0" />

- Getting the plot between Vin and Vout by connecting the interaction points, and we get the information that PMOS and NMOS are working in which region as well.
<img width="900" height="285" alt="image" src="https://github.com/user-attachments/assets/050bda8f-1e04-4296-8865-92753ce4c0fc" />
<img width="400" height="325" alt="image" src="https://github.com/user-attachments/assets/ef9046db-02c8-46fc-8ef5-0f3e1701fae0" />

## D) Day 3 - CMOS Switching threshold and dynamic simulations

### Voltage transfer characteristics – SPICE simulations
### L1: SPICE deck creation for CMOS inverter
- This sessions explain the spice deck construction

- Spice deck contains
  1. Circuit connectivity information
  2. Transistor definitions
  3. Input and supply voltages
  4. Load capacitance
  5. Output observation points
  
The deck completely defines the circuit for simulation.

- The substrate affects threshold voltage (body effect), so it is explicitly defined.

Clearly define nodes for SPICE:
   - *in → Input node*
   - *out → Output node*
   - *VDD → Supply node*
   - *0 → Ground (VSS)*
- A node is defined as a connection point between components. Proper node naming is essential for correct simulation. It is also important to identify the minimum number of nodes required to connect these devices.
<img width="300" height="95" alt="image" src="https://github.com/user-attachments/assets/5dbbd082-65af-44ca-8414-e0606791c577" />

<img width="400" height="390" alt="image" src="https://github.com/user-attachments/assets/e0d60e07-c13e-4a64-b69f-00bee243bd87" />

### L2: SPICE simulation for CMOS inverter
Now netlist is being defined for the full system. It contains PMOS, NMOS, Capacitor Load, Vdd, and Input voltage.

- First, we are defining the components and then the simulation commands.

Taking w=0.9 and L=0.6 for both NMOS and PMOS in the inverter.
- I have taken from V = 0 to 2.5 with keeping step size of 0.05V.
<img width="300" height="295" alt="image" src="https://github.com/user-attachments/assets/89eeeb4e-1e34-468e-8650-7280710014a2" />

Taking w=0.9, L=0.6 for NMOS and w=3, L=0.6 for PMOS in the inverter. Here w/L=1.5 for NMOS and w/L=5 for PMOS.
- I have taken from V = 0 to 2.5 with keeping step size of 0.05V.
<img width="300" height="295" alt="image" src="https://github.com/user-attachments/assets/132850a4-7b65-4240-bd12-4af269842d74" />

- I had understood from the w and L values that large w and L values mean an increase in capacitance, resulting in an increase in delay. The circuit becomes slower.

### L3: Labs Sky130 SPICE simulation for CMOS
- Adding the point of switching threshold in this lecture.

For the calculations done in the previous lecture, the approximate value switching threshold = 1.07479V.

- Transient Analysis
By running ngspice for transient analysis, we can get the rise and fall times, from which we can calculate the delay.

<img width="300" height="295" alt="image" src="https://github.com/user-attachments/assets/6dd6a79d-32ec-4718-a8fe-a87aee818b1a" />

- Going to see at the point of 50% voltage rise, at V=0.9, I get delay = 2.4833 - 2.1511 = 0.3321 ns.
- Similarly for fall delay, I got 4.3360 - 4.0495 = 0.2865 ns

### Static behavior evaluation – CMOS inverter robustness – Switching Threshold
### L1: Switching Threshold, Vm
- By comparing those devices from L2: spice simulation for CMOS, we get to know that it is a robust device. No matter how large PMOS is and how small NMOS is, at low Vin, PMOS will be on, and at high Vin, NMOS will be on.
<img width="900" height="400" alt="image" src="https://github.com/user-attachments/assets/0a819f3d-da22-4445-81df-f8dc87aa7622" />
<img width="900" height="430" alt="image" src="https://github.com/user-attachments/assets/a4a90b06-16e2-4499-b44b-4703d043357b" />

Following that, Vin = Vout straight line intersection will be our switching voltage point
- Now, as we know that for a higher PMOS width device, it has a higher switching voltage, and depending on the w/L ratio, the switching voltage changes.
- We also get to know that there is a critical area in the Vin vs Vout curve we view, where the leakage of the device is dependent on.

### L2: Analytical expression of Vm as a function of (W/L)p and (W/L)n
Current evaluation at Vin=Vout or Vgs=Vds
- IdsP = - IdsN
- Idsp + IdsN = 0

Further deriving the expression as attached in the image.
<img width="900" height="390" alt="image" src="https://github.com/user-attachments/assets/ead660cf-8442-4e29-8cd5-b30cb098e447" />
![WhatsApp Image 2026-02-24 at 10 26 30 PM](https://github.com/user-attachments/assets/31227712-6e92-4d42-b905-33d10fe404e5)

<img width="867" height="271" alt="image" src="https://github.com/user-attachments/assets/f14908b8-4e8a-4690-89f6-90991cbf167f" />

- kp and kn are gain factors
- R is the process transconductance

### L3: Analytical expression of (W/L)p and (W/L)n as a function of Vm
![WhatsApp Image 2026-02-24 at 10 43 29 PM](https://github.com/user-attachments/assets/8c062c32-a186-4959-a1ed-ad427d6ededc)

### L4: Static and dynamic simulation of CMOS inverter
- Here we have similar calculations as in Lecture 3 of Day 3. After transient calculations with a pulse instead of DC, we calculated the time delay.
<img width="900" height="450" alt="image" src="https://github.com/user-attachments/assets/cf6b04e9-b668-4c42-9854-d2d2c72bb573" />

### L5: Static and dynamic simulation of CMOS inverter with increased PMOS width
- wp=1, Lp=0.25, Wn=1, Ln=0.25
- Rise delay = 2.6694 - 2.1506 = 0.5188 ns
- Fall delay = 4.2116 - 4.0502 = 0.1614 ns
- Vm = 0.65V
<img width="400" height="380" alt="image" src="https://github.com/user-attachments/assets/c7048e71-5126-40a6-904b-545ce39188e8" />

<img width="400" height="385" alt="image" src="https://github.com/user-attachments/assets/79b99a09-74c9-443c-8e14-57c0995fe0bc" />

- wp=2, Lp=0.25, Wn=1, Ln=0.25
- Rise delay = 2.4270 - 2.1495 = 0.2775 ns
- Fall delay = 4.2163 - 4.0494 = 0.1669 ns
- Vm = 0.684V
<img width="400" height="385" alt="image" src="https://github.com/user-attachments/assets/f6fd9c23-a135-4ed9-8f7c-8404bdaeda92" />

- wp=3, Lp=0.25, Wn=1, Ln=0.25
- Rise delay = 2.3473 - 2.1493 = 0.198 ns
- Fall delay = 4.2219 - 4.0489 = 0.173 ns
- Vm = 0.71V
<img width="400" height="360" alt="image" src="https://github.com/user-attachments/assets/a28f8e95-55b0-49fa-899d-5409ffdc862f" />

<img width="400" height="375" alt="image" src="https://github.com/user-attachments/assets/aaa6caf4-3b2d-4327-959e-efe62d7330f9" />

- wp=4, Lp=0.25, Wn=1, Ln=0.25
- Rise delay = 2.3090 - 2.1494 = 0.1596 ns
- Fall delay = 4.2236 - 4.0493 = 0.1743 ns
- Vm = 0.722V
<img width="400" height="360" alt="image" src="https://github.com/user-attachments/assets/43818e49-df8b-435c-b7d7-c7e508895006" />

<img width="400" height="370" alt="image" src="https://github.com/user-attachments/assets/abefd56c-d744-4bf0-997e-9a51bab1f2f9" />

- wp=5, Lp=0.25, Wn=1, Ln=0.25
- Rise delay = 2.2854 - 2.1505 = 0.1349 ns
- Fall delay = 4.2264 - 4.0505 = 0.1759 ns
- Vm = 0.73V
<img width="400" height="360" alt="image" src="https://github.com/user-attachments/assets/16d81640-4230-4af8-8917-0d2065e688c2" />

<img width="400" height="370" alt="image" src="https://github.com/user-attachments/assets/c34ec3f0-db25-4e99-9358-0f2ebf5aeac6" />

### L6: Applications of CMOS inverter in clock network and STA
<img width="1000" height="575" alt="image" src="https://github.com/user-attachments/assets/de79aedd-6a2f-400b-9a48-4a3abfb4b47d" />

- We are checking the point where our rise and fall delay is the same. Here we can see the buffer with the same R to have a similar delay in rise and fall.

## E) Day 4 - CMOS Noise Margin robustness evaluation

### Static behavior evaluation – CMOS inverter robustness – Noise margin

### L1: Introduction to noise margin
- Noise margin is an important aspect of the device; it might create a disturbance in logic levels.

- Considering the Ideal Inverter, we will have the following response, as shown in the image.
- When Vin = 0(0), Vout = Vdd(1)
- Vin = Vdd(1), Vout = 0(0)
- Switching happens at Vdd
- slope = Vdd/0 = Infinite

<img width="586" height="535" alt="image" src="https://github.com/user-attachments/assets/e3ffcb5f-c846-483a-89fb-508675d42852" />

- In a real device, there is a finite slope due to finite resistance and parasitic capacitance.
- VOH Output High Voltage = Maximum output voltage (near VDD)
- VOL Output Low Voltage = Minimum output voltage (near 0 V)

- Maximum input low is still recognized as Logic low => 0 ≤ Vin ​≤ VIL
- Minimum input high is still recognized as Logic high => VIH​ ≤ Vin​ ≤ VDD
- We have a Transition region between VIL and VIH
​
<img width="900" height="500" alt="image" src="https://github.com/user-attachments/assets/efd109df-7006-41e3-9f16-393cb08a4a9f" />

- The steeper the slope of the voltage transfer characteristic, the better the switching behavior and the higher the noise immunity; an ideal inverter has an infinite slope at the switching point, which provides the best noise rejection, whereas a practical inverter has a finite slope due to actual device effects, resulting in limited noise immunity.

### L2: Noise margin voltage parameters
- The input voltage is split into three sections by the CMOS inverter VTC: the logic 0 region, the transition region, and the logic 1 region.
- Logic 0 is defined as any input voltage between 0 and VIL.
- Logic 1 is defined as any input voltage between VIH and VDD.
- There is an indeterminate transition zone between VIL and VIH.
- The output becomes VOL (output low) when the input is in the logic 1 region.
- The output becomes VOH (output high) when the input is in the logic 0 region.
- Because of device non-idealities, VOL is not precisely 0 V, and VOH is not precisely VDD in real circuits. For logic gates to cascade properly, VOL < VIL and VOH > VIH.

<img width="900" height="450" alt="image" src="https://github.com/user-attachments/assets/8a0fab3d-1707-45cb-a479-1572569e9aa3" />

- These requirements guarantee that logic 0 and logic 1 are successfully detected in the subsequent stage. Due to parasitic capacitance and limited resistance, the realistic VTC curve is smooth rather than vertical.

- Because an increase in input results in a reduction in output, the transition region's slope is negative. Near the switching point, the slope's magnitude is almost -1.

### L3: Noise margin equation and summary
- Plotting the 1D line graph for the Noise Margin.
- Plotting these voltage levels reveals two significant regions:

1. Noise Margin High (NMH)
- It is the voltage difference between VOH and VIH.
- NMH = VOH−VIH
- When the signal is at logic 1, it indicates the maximum acceptable noise level. At the input or output, any voltage within this range will still be recognized as a logic 1.

2. Noise Margin Low (NML)
- It is the voltage difference between VIL and VOL.
- NML = VIL − VOL
- When the signal is at logic 0, it indicates the maximum acceptable noise level. Logic 0 will still be recognized for any voltage falling inside this range.

3. Undefined region
- The undefined region is the area between VIL and VIH. It interprets any voltage occurring inside this range as either logic 0 or logic 1.
- Uncertainty arises when signals or glitches enter this area. When operating a circuit properly, this area must be avoided.

<img width="1131" height="567" alt="image" src="https://github.com/user-attachments/assets/4594407e-24c2-4402-82b7-a11286bdee3f" />

- Noise margin gives the idea of tolerance in the circuit. It helps determine whether a bump or glitch is safe or unsafe.

<img width="1167" height="734" alt="image" src="https://github.com/user-attachments/assets/fb8905cf-4ce0-4f73-adc2-6dd3bb278202" />

### L4: Noise margin variation with respect to PMOS width
To evaluate noise margins through PMOS/NMOS size scaling, CMOS inverter noise margins are analyzed using Spice simulations.

<img width="1133" height="384" alt="image" src="https://github.com/user-attachments/assets/8ab77fb5-0df0-44aa-9c8a-3c42b2a9bc1e" />

- Checking the slope of the DC transfer curve and then defining the range of voltages, which are noise margins for the respective device.

- Because of NMOS, increasing PMOS size increases the noise margin high from 0.3V → 0.42V at 5x w/L of NMOS, but there is a slight decrease in the noise margin low beyond 3x w/L of NMOS.

<img width="900" height="780" alt="image" src="https://github.com/user-attachments/assets/038f62c5-f116-4b83-b709-0f351d9369bf" />
<img width="900" height="780" alt="image" src="https://github.com/user-attachments/assets/8706211f-f63f-499b-a29c-4a55b3dbce23" />

- Fabrication variations cause noise margin changes in digital circuits.

- Therefore, CMOS inverters demonstrate robustness against noise within optimal size ratios, making them suitable for digital design where minor variations (±5%) in margins do not compromise functionality.

### L5: Sky130 Noise margin labs
ngspice simulations of the noise margin with a load of 50 fF at out.
- Here I got noise margin
- High = VIH - VOH = 1.7 - 0.98 = 0.72
- Low = VIL - VOL = 0.78 - 0.11 = 0.67

<img width="500" height="460" alt="image" src="https://github.com/user-attachments/assets/27309a68-1432-4834-8c75-b1db67b1c274" />

## F) Day 5 - CMOS power supply and device variation robustness evaluation

### Static behavior evaluation – CMOS inverter robustness – Power supply variation
### L1: Smart SPICE simulation for power supply variations
- In this part, we have discussed creating the netlist for evaluating CMOS inverter robustness under power supply scaling.
- SPICE variable loops are used to sweep the supply voltage in 0.5V increments (2.5V → 0.5V).
- To account for variations in mobility and preserve switching symmetry, we keep PMOS broader than NMOS.

<img width="1000" height="575" alt="image" src="https://github.com/user-attachments/assets/29c13cd5-c174-451b-86c6-b994dd284241" />

### L2: Advantages and disadvantages using low supply voltage

Advantages
- We can get 50% improvement in gain for a 0.5V device as compared to 2.5V.
- Energy needed for charging and discharging is 90% less for a 0.5V device with respect to 2.5V device.

<img width="1000" height="500" alt="Screenshot 2026-02-25 155507" src="https://github.com/user-attachments/assets/28d2fd76-3f7c-4f40-aafa-88e83789bbd0" />

Disadvantage
- Generally, we think that a lower power consumption and a higher gain device perform better, but that device is not getting enough time to fully charge and discharge, resulting in a longer time as compared to a 2.5V device.

| 2.5V device | 0.5V device |
|---|---|
| <img width="400" height="320" alt="Screenshot 2026-02-25 160254" src="https://github.com/user-attachments/assets/fc080aa7-f8fe-4d73-886f-c53496730db7" /> | <img width="400" height="320" alt="Screenshot 2026-02-25 160414" src="https://github.com/user-attachments/assets/b7438da8-503a-48c0-a393-5fe69c846a50" /> |

### L3: Sky130 Supply Variation Labs
- Spice simulation is done for analyzing the gain with respect to the width of the device.

- For V=1.8V, Gain = 8.33
- For V=1.6V, Gain = 8.43
- For V=1.4V, Gain = 9.06
- For V=1.2V, Gain = 9.82
- For V=1V, Gain = 9.02
- For V=0.8V, Gain = 8.46

<img width="500" height="490" alt="Screenshot 2026-02-25 161249" src="https://github.com/user-attachments/assets/1eb1ddaa-a065-41a0-bbd5-ded061a17137" />

### Static behavior evaluation – CMOS inverter robustness – Device variation
### L1: Sources of variation – Etching process
- Transistor geometry, including gate length (L) and width (W), is defined by the crucial fab process of etching.

- The MOSFET drain current is directly impacted by the w/L ratio.

- Well, depending on the arrangement around, variations may vary from one chip to another.
<img width="900" height="300" alt="Screenshot 2026-02-25 164609" src="https://github.com/user-attachments/assets/013ae379-c4fa-4218-baeb-f72e7f3073f2" />

- The inverter's switching latency is modified by variations in drain current. And because of their homogeneous adjacent structures, central inverters frequently exhibit more periodic and similar distortions, whereas edge inverters generate uneven fluctuations due to different nearby layout architectures.

- The timing and reliability of larger circuits are affected by variations that accumulate along inverter chains.

<img width="1000" height="525" alt="Screenshot 2026-02-25 164944" src="https://github.com/user-attachments/assets/cdfaa2ab-f844-4cfc-ab0f-845b0f1ac4a3" />

### L2: Sources of variation – oxide thickness
- Similar to etching, here in oxidation, it also varies from the ideal case due to the real nature of Fab.

| Ideal | Real |
|---|---|
|<img width="538" height="157" alt="Screenshot 2026-02-25 170306" src="https://github.com/user-attachments/assets/ed45bd9b-295b-4298-a5da-8184fdb7f133" /> | <img width="531" height="133" alt="Screenshot 2026-02-25 170406" src="https://github.com/user-attachments/assets/c42ce779-7914-4330-afef-119a55ad3c38" />|

- Here, too, there is a similar variation in central COMS devices and uneven variation in oxide thickness due to different nearby layout architectures.

- Eventually, Id is dependent on Cox, which is dependent on oxide thickness(tox), resulting in a change of Id, which might not be acceptable.

<img width="1281" height="152" alt="Screenshot 2026-02-25 170701" src="https://github.com/user-attachments/assets/07b65f3c-6ab9-45ac-85f6-cd2f06d06f25" />

### L3: Smart SPICE simulation for device variations
- Now sweeping NMOS and PMOS from weak NMOS to strong NMOS. Here we are checking the robustness of the CMOS inverter.

<img width="1722" height="304" alt="Screenshot 2026-02-25 172440" src="https://github.com/user-attachments/assets/f44ffba3-8167-4836-aaae-34eaac1ff4d2" />

- Analysing the noise margins, we can see that whenever there is a Logic High needed, there is enough margin to capture the noise. It doesn't change much with the w and L variation. 

<img width="900" height="700" alt="Screenshot 2026-02-25 172811" src="https://github.com/user-attachments/assets/3b154d31-774f-49c0-8dfc-9b78e28298c0" />


### L4: Conclusion
- The switching threshold varied by 1.2V (2.0V to 1.4V) but remained within acceptable limits relative to the supply voltage.
- Noise margins of 400mV (strong PMOS) and 300mV (weak PMOS) are sufficient to filter supply noise and ground bounce.
- Inverter behavior preserved across extreme device variations (strong-to-weak PMOS/NMOS), ensuring reliable logic functionality.

<img width="900" height="700" alt="image" src="https://github.com/user-attachments/assets/c94bb4c7-012e-4a30-a01d-8be0a60ef70f" />

<img width="900" height="700" alt="Screenshot 2026-02-25 173345" src="https://github.com/user-attachments/assets/ec434d04-b79d-496c-8dc0-171e9ebd3f15" />

