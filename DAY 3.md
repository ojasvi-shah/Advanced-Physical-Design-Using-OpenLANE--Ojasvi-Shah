![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/8a998f76-cef1-479f-84b6-19bfed02b4a4)# SKY130 DAY 3: Design Library Cell Using Magic Layout and NGSPICE characterisation
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


## Inception of Layout Â CMOS Fabrication Process
### Create Active Regions
### Formation of N and P well
### Formation of Gate Terminal
### Lightly Doped Drain [LDD] Formation
### Source Â Drain Formation
### Local Interconnect Formation

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
