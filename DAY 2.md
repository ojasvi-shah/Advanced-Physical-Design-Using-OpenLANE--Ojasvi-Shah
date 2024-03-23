# SKY130 Day 2: Good vs Bad Floorplan and Introduction to Library Cells
## Chip Floor Planning Considerations
### Utilisation Factor and Aspect Ratio

The first step in physical design is to **define the width and height of the core and die** : Beginning with a very simple netlist, that can extrapolated later we will first draw a basic diagram in the form of symbols that we will later convert into physical designs. We will take each cell (gates, specific cell like flip flop) and give it a standard (although rough for now) dimensions. As an example here, each unit will be 1 unit x 1 unit - i.e. 1 sq. unit in size, and since there are 4 gates/flip-flops here, the total size of the silicon wafer will 4 sq. units.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/bd876662-ea2d-4ca0-9393-39e2e34bdec2)


> NOTE : here, we are ignoring the wires

All logical cells will be placed inside the core - which is part of the die. If the logical cells occupy the core fully, it is known as 100% utilisation. Utilisation factor = Area occupied by netlist / Total area of core. In this case, we see that utilisation factor is 100%, but practically it is usually 50%. Aspect Ratio is the ratio between height and width. If the chip is square - it is 1, else the chip is rectangular in shape.

Example: 
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/056fe880-4a00-4941-aab1-8e22c09e52c2)

Utilisation factor = 50 %

Aspect Ratio = 2 : 4 = 1 : 2 = .5


### Concept of Pre-Placed Cells

Pre-Placed cells are complex logic blocks that can be reused. They are already implemented and cannot be touched by Auto Place and Route tools - and hence are required to be very well designed. Placement of such cells are user-based. A combinational logic - such as netlist shown does a particular function and is composed of various gates. We can divide this logic into blocks - while preserving the connectivity of the logic. By extending IO pins and making connections we can convert the logic into two parts - that are blackboxed and can be used as needed. If a design only requires a black box, it can be directly handed over to the designer with out much hassle. The various preplaced blocks available include memory, clock-gating cell, comparator, MUX. The arrangement of these IPs in a chip are known as floorplanning.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/960cb6af-73a6-4fe9-ba92-5d658b41b8fb)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/503e2a1f-ce46-4790-9260-6a2ce2b6f1ec)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ea9c5c91-9634-4a63-b022-24969a98a2d6)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/2c193af2-6ff9-4533-8d6c-0c2e1833283f)


### De-Coupling Capacitors

We surround pre-placed cells with de-coupling capacitors. If we think of a circuit to be part of a block, whenever it is switched on there is a demand for current, which is supplied by the Vdd. Upon switching the circuit off, there is a discharge, which the ground accepts. However, practically when voltage is supplied is passes through a wire which causes it to reduce slightly due the resistance, inductance and capacitance in the wire, and the reduced voltage is called Vdd'. The Vdd' always needs to stay in the noise margin - which ranges from Vih to Voh. If this is not true, the circuit is unstable. This is due to the large physical distance between the actual voltage supply and the circuit.

Decoupling capacitors is a solution to this problem. Decoupling capacitors can be thought of as as a huge capacitor completely filled with charge. The equivalent voltage across the capacitor is same as across the main supply voltage. The capacitor decouples the circuit from the main supply. Hence, all the pre-placed cells get their power supply from the capacitors and hence are completely stable.

### Power Planning

Consider a particular piece of logic to be a _macro_, that is repeated many times on a single chip. It requires a lot of voltage, so voltage must be supplied through a decoupling capacitor. However, it is not feasible to add the de-coupling capacitors on the entire circuit - only critical elements can be decoupled.

If the 16 bit bus is connected to an inverter, then it means that all the capacitors will discharge the voltage at once. A lot of capacitors discharging at once cam cause **Ground Bounce** due to great amount of voltage needed to drained at the same time, and turning the capacitor on might cause **Voltage Drop** due to insufficient current. Ground bounce and voltage drop might cause the voltage to not be within the noise margin range. To solve this problem, we can have multiple powersource taps and sources ( known as a power mesh) where capacitors can source current from the nearest Vdd and sink current to the nearest Ground.

### Pin Placement and Logical Cell Placement Blockage

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/92ccc847-3206-4223-b279-f73045b7d0e5)

Take the above netlist as an example needing to be implemented. The information about the connections is coded through VERILOG (also known as VHDL).The input and output ports are placed left and right between the core and the die respectively. The placements of the ports is cell-specific. The clocks are continuously drive the cells and hence clock ports are bigger than data ports. Due to this, we also need the least resistance paths for the clocks. The size is inversely proportional to the resistance.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/2d72b528-89f5-42d4-bb11-5a183b37113b)

After the pin placement, we create Logical Cell Placement Blockage to ensure that the APR tool does not place any cell on the pin locations.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f87544bd-7fb1-422d-9229-d885444d6c89)


### Steps to Run Floorplan Using OpenLANE

1. The first step is setting the configuration variables - Before running floorplan, the configuration variables or switches must be set. These are present in **openlane/configuration**

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/64423e9c-e562-45ee-a035-3744dc8db0a3)

The _README.md_ contains all configuration variables, which are segregated based on stage and the **.tcl** files consists of the default OpenLANE settings.

2. All _configurations/switches_ accepted by the current run are from **openlane/designs/[design - date]/config.tcl**

                                  ----------------

There is a order of priority -:

- _openlane/designs/[design-date]/sky130A_sky130_fd_sc_hd_config.tcl_
- _openlane/designs/[design]/config.tcl_
- _openlane/configuration/floorplan.tcl_

> In OpenLANE, it is important to note that the vertical and horizontal metals set one more than what we specify. For example, if the vertical metal is specified as 3, then it'll be 4.

3. Floorplan is to be run on OpenLANE through the command :- _run_floorplan_

### Review Floorplan Files and Steps to Review Floorplan

4. After running floorplan as above, it will produce a result that will be stored in the form of a design exchange format - and will contain the area of the Die as well as positions.The die area in this file is in database units and 1 micron is equivalent to 1000 database units. Area of die = (554570/1000) microns * (565290/1000) microns = 311829.1653 sq. um.

### Review Floorplan Layout in Magic

5. The command _magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &_ should be typed to view the file.
6. Subsequently, press the **S** key to select the entire die and then **V** to center the view, and then **Z** to zoom. You will observe that the IO pins are placed equidistant to one another in a random mode as based on the configuration **(FP_IO_MODE = 1)** set in _openlane/configuration/floorplan.tcl_

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/d38b08d1-e7ca-4138-b10d-1c31b0ffc6d7)

7. After this, typing _what_ on the **tkcon** window will give the layer of the selection.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/141544b1-4568-4c34-b348-c3b9138ffaa1)

> Standard cells are not placed but can be viewed at the bottom left corner of the layout
>![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/5dcd79af-393d-4d18-8986-0fe000c72db7)

    
## Library Binding and Placement
### Netlist Binding and Initial Place Design
### Optimise Placement Using Estimated Wire-Length and Capacitance
### Final Placement Optimization
### Need for Libraries and Characterisation
### Congestion Aware Placement Using RePLACE
    
## Cell Design and Characterisation Flows
### Inputs for Cell Design Flow
### Circuit Design Step
### Layout Design Step
### Typical Characterisation Flow
     
## General Timing Characterisation Parameters
### Timing Threshhold Definitions
### Propogation Delay and Transition Time
