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


### Steps to Run Floorplan Using OpenLANE
### Review Floorplan Files and Steps to Review Floorplan
### Review Floorplan Layout in Magic
    
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
