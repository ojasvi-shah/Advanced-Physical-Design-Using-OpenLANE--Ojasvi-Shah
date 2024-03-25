# SKY130 DAY 3: Design Library Cell Using Magic Layout and NGSPICE characterisation
## Labs for CMOS Inverter NGSPICE Simulations
### IO Placer Revision

OpenLANE configurations can be changed inside the shell itself, on the fly. IO Mode is usually set to _random equidistant_. However, if we want to change this, we can do so through the following command typed after floorplan : **set ::env(FP_IO_MODE) 2**. After running this command, the IO [input - output] pins will not be equidistant in mode 2 (instead of the default - that is 1). 

After this, we may re-run floorplan, and then check by seeing that the pins are placed based on of Hungarian algorithms now i.e. stacked one over the other. 

> NOTE: changing the configuration on the fly will not change the runs/config.tcl, the configuration will only be available on the current session.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/64fccafc-180a-4e66-881a-80fabe083dc1)

### SPICE Deck Creation For CMOS Inverter

The SPICE deck contains connectivity information about netlists, inputs to be provided, TAPS for the outputs etc. The component values are taken, that are usually -: for the PMOS it is .375u/.25u (i.e. the channel length is .25 micron and and the channel width is .375 micron). Ideally, the PMOS should be 2 to 3 times wider than the NMOS. This is as the PMOS hole carrier is slower than the NMOS carrier, and since the rise and fall time must be matched, to reduce the resistance, we increase the width of the PMOS. The next steps are to identify and name the nodes:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e85697ff-266b-4fc5-83a9-c3fe0143ffcf)

The syntax of the SPICE deck netlist PMOS and NMOS is _[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]_. It is to be noted that all components in a netlist are described based on its node and values.

> EXAMPLE SYNTAX - M1 OUT IN VDD VDD PMOS W=.375U L=.25u

### SPICE Simulation Lab for CMOS Inverter

The start of SPICE simulation is _.op_ where in Vin will be swept from 0 to 2.5 with 0.05V steps. The model file is **tsmc_025um_model.mod** that has all the technological parameters for the 0.25um NMOS and PMOS.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/bdec7a54-4667-4c1f-acbc-193062f2bcda).

For SPICE simulation, there are various steps-:

1) Open the NGSPICE simulator
2) Source the Circuit File through _source_ command
3) Execute it by the command _run_ and then use _setplot_ which allows one to view any plots possible from the simulations specified in the spice deck and will give you a choice for which simulation to be run
4) Then, type _display_ which will give you a choice of nodes to be plotted which when _plot out vs in_ is typed will be plotted on a graph.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/a4931bc2-cb5d-4e9e-8c14-54bc916c0b00)

### Switching Threshhold Vm

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/da38d8fa-1309-485b-b68d-e0e35c819a0a)

POINTS TO BE NOTED:

- The shapes of the graphs are almost the same, through which we can derive the conclusion that CMOS is a robust device
- The parameters that determine the robustness of the CMOS is the switching threshhold and the propogation delay

The Switching Threshhold is the point where the the input voltage is equal to the output voltage and both PMOS & NMOS are in saturation region. When these are turned on, there is a high chances of leakage and  that the current flows directly from VDD to GND. Due to this, short circuit can be seen.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/07f581e6-f0c1-49b3-b190-18c4d5a05157)

### Static and Dynamic Simulation of CMOS Inverter

To find Vm, we use DC TRANSFER ANALYSIS. Simulation is essentially a sweep from 0V to 2.5V by taking 0.05V steps.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/7709cad2-6227-4878-baad-d3165745ef67)

To find propogation delay, we use transient analysis when a pulse is applied to the CMOS.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1797489d-2861-4149-879b-496c616550db)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9dda14bc-11c3-4469-9d27-4ad812765df2)

### Lab Steps to GitClone VSDSTD Cell Design

We have been provided with a github repository wherein inverter files lie. It is available at this link - https://github.com/nickson-jose/vsdstdcelldesign. Steps to clone and observe the layout are as follows:
1. Clone the custom inverter standard cell design from the github repository shared above
2. Clone the repository with the custom inverter design through the command _git clone https://github.com/nickson-jose/vsdstdcelldesign_
3. Subsequently, copy the tech file to the _vsdstdcelldesign_ directory (created through above step) by this command _cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/_
4. Then, open the custom inverter layout in MAGIC through this command: _magic -T sky130A.tech sky130_inv.mag &_cp__

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/307eb43f-4fe3-4d28-bca0-4e495d171489)

## Inception of Layout and CMOS Fabrication Process

The 16 MASK CMOS Fabrication process is as follows:

### Create Active Regions

1. The first step is to select a substrate - which is where the entirety of your design is fabricated. The most common substrate is a P doped Silicon Substrate. A substrate is ideally lesser doped than it's wells.

2. The next step is creating an active region for transistors. It is to be noted that it is necessary to have isolation between the pockets, which can be done through
   - Growing 40nm of Silicon Dioxide
   - Depositing 80nm of Silicon Nitride.
   - Depositing a layer of photoresist
   - Deposit mask-1 layer on top of photoresist. It covers the photoresist layer that must not be etched away (protects the two transistor active regions)
   - Applying UV light to remove the layers on the unmasked regions
   - Removing mask-1 and photoresist layers
   - Placing the chip in the furnace to grow the oxide in other areas
   - Removing the Si3N4 layer using hot phosphoric acid to have only p-substrate and SiO2 left

### Formation of N and P well

3. P well and N well formation
    + Deposition of photo resist layer and define the areas to protect by deposition of mask-2 and 3. Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated and Mask 3 protects P-Well while N-Well is being formed
    + Application of UV Light to remove the exposed photoresist
    + Placing of chip in furnace to diffuse the boron and phosphorous to form wells. This process is called **Twintub** process.

> Boron [B] is used to form P-Well and Phosporus [P] is used to form N-well

### Formation of Gate Terminal

Gate Terminal is where Threshhold Voltage is controled - as seen below:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/2803bfef-5a63-4e51-a91f-ca5f3bd2f3c5)

4. Formation of Gate
     + Deposit photo resist layer to define the areas to be protected, and then subsequently deposit mask-4. Then, UV light is applied, and the exposed area of photoresist is removed
     + Then, implantation of  low energy boron at the surface of p-well using mask-4 to control the threshold occurs
     + Similarly, implantation of phosphorous/arsenic for n-well using mask-5 occurs
     + Fixing the oxide which is damaged by implantation steps by removing extra SiO2 using the hydroflouric acid and re-grow high quality SiO2 on p-substrate to contol the oxide thickness occurs next
     + Addition of polysilicon film subsequently occurs
     + Then, mask-6 is added and etching using photolithography occurs
     + Then, mask 6 is etched off to form the gate terminal

### Lightly Doped Drain [LDD] Formation

5. LDD Formation - the reason LDDs are created is to prevent the hot electron which can eventually cause Si - Si bonds break or create voltage that passes the 3.2eV barrier leading to issues with doped regions. The second major need is to prevent another effect, known as the short channel effect which can cause gate malfunctioning due to the drain field penetrating the channel.
     + Mask 7  and 8 are created for NMOS (lightly doped N-type) and PMOS (lightly doped P-type) respectively.
     + Heavily doped impurity (N+ for NMOS and P+ for PMOS) are added for the actual source and drain but the lightly doped impurity which are also added help maintain spacing between the source and drain and prevent hot electron effect and short channel effect.
     + To protect the lightly doped regions, we also add SiO2 and create spacers using _plasma anisotropic_ etching

### Source and Drain Formation

6. Source and Drain Formation
     + Thin screen oxide is added to avoid channeling during. Channeling is when implantations dig too deep into substrate which is very problematic
     + We create Mask-9 is for N+ implantation and Mask-10 for P+ implantation
     + The side wall spacers maintain the N-/P- while implanting the N+/P+
     + High temperature annealing is done as well

### Local Interconnect Formation

7. Steps to Form Connects and Interconnects [LOCAL] - these are very important as they help in controlling the electrical charecteristics. These are also the only things accessible to the end user.
     + The thin screen oxide is removed for opening up the source, drain and gate for contact building. We use Titanium as it has less resistance.
     + TiSi2 is used for local interconnects
     + Mask 11 is formed and TiN is etched off by RCA cleaning to create the first level contact

### Higher Level Metal Formation
### Lab Introduction to SKY130 Basic Layers Layout and LEF using Inverter
### Lab Steps to Create STD Cell Layout and Extract SPICE Netlist

## SKY130 Tech File Labs
### Lab Steps To Create Final SPICE deak using SKY130 tech
### Lab Steps to Characterise Inverter using SKY130 Model Files
### Lab Introduction to SKY130 PDKs And Steps to Download Labs
### Lab Introduction to Magic Tool Options and DRC Rules
### Lab Introduction to Magic and Steps to Load SKY130 Tech Rules
### Lab Excercise to Fix **poly.9** error in SKY130 Tech-File
### Lab Excercise to Implement Poly-Resistor Spacing to Diff and Tap
### Lab Challenge Excercise To Describe DRC Error as Geometrical Construct
### Lab Challenge to Find Missing or Incorrect Rules and Fix Them
