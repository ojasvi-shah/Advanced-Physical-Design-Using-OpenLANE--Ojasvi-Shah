# Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah

Course -  [Advanced Physical Design Using OpenLANE/SKY130 offered by VSDIAT](https://vsdsquadron.vlsisystemdesign.com/digital-vlsi-soc-design-and-planning/)

Author - Ojasvi Shah

Contents -:
* [SKY130 Day 1: Inception of OpenSource EDA, OpenLANE and SKY130 PDK](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md)
    - [How to Talk to Computers](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#how-to-talk-to-computers)
        + [Introduction to QFN - 48 package, chip, pads, core, die and IPs](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#introduction-to-qfn---48-package-chip-pads-core-die-and-ips)
        + [Introduction to RISC V](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#introduction-to-risc-v)
        + [From Software Applications to Hardware](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#from-software-applications-to-hardware)
    - [SoC Design and OpenLANE](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#soc-design-and-openlane)
        + [Introduction to Components of Opensource Digital ASIC Design](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#introduction-to-components-of-opensource-digital-asic-design)
        + [Simplified RTL to GDS flow](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#simplified-rtl-to-gds-flow)
        + [Introduction to OpenLANE and Strive Chipsets](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#introduction-to-openlane-and-strive-chipsets)
        + [Introduction to OpenLANE detailed ASIC Design Flow](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#introduction-to-openlane-detailed-asic-design-flow)
    - [Get Familiar to Opensource EDA tools](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#get-familiar-to-opensource-eda-tools)
        + [OpenLANE Directory Structure in Detail](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#openlane-directory-structure-in-detail)
        + [Design Preparation Step](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#design-preparation-step)
        + [Review Files After Design Prep and Run Synthesis](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#review-files-after-design-prep-and-run-synthesis)
        + [Steps to Characterise Synthesis Results](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%201.md#steps-to-charecterise-synthesis-results)        
* [SKY130 Day 2: Good vs Bad Floorplan and Introduction to Library Cells](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#sky130-day-2-good-vs-bad-floorplan-and-introduction-to-library-cells)
    - [Chip Floor Planning Considerations](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#chip-floor-planning-considerations)
        + [Utilisation Factor and Aspect Ratio](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#utilisation-factor-and-aspect-ratio)
        + [Concept of Pre-Placed Cells](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#concept-of-pre-placed-cells)
        + [De-Coupling Capacitors](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#de-coupling-capacitors)
        + [Power Planning](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#power-planning)
        + [Pin Placement and Logical Cell Placement Blockage](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#pin-placement-and-logical-cell-placement-blockage)
        + [Steps to Run Floorplan Using OpenLANE](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#steps-to-run-floorplan-using-openlane)
        + [Review Floorplan Files and Steps to Review Floorplan](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#review-floorplan-files-and-steps-to-review-floorplan)
        + [Review Floorplan Layout in Magic](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#review-floorplan-layout-in-magic)
     - [Library Binding and Placement](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#library-binding-and-placement)
        + [Netlist Binding and Initial Place Design](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#netlist-binding-and-initial-place-design)
        + [Optimise Placement Using Estimated Wire-Length and Capacitance](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#optimise-placement-using-estimated-wire-length-and-capacitance)
        + [Final Placement Optimization](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#final-placement-optimization)
        + [Need for Libraries and Characterisation](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#need-for-libraries-and-characterisation)
        + [Congestion Aware Placement Using RePLACE](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#congestion-aware-placement-using-replace)
    - [Cell Design and Characterisation Flows](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#cell-design-and-characterisation-flows)
        + [Inputs for Cell Design Flow](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#inputs-for-cell-design-flow-and-circuit-and-layout-design-step)
        + [Circuit Design Step](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#inputs-for-cell-design-flow-and-circuit-and-layout-design-step)
        + [Layout Design Step](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#inputs-for-cell-design-flow-and-circuit-and-layout-design-step)
        + [Typical Characterisation Flow](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#typical-characterisation-flow)
     - [General Timing Characterisation Parameters](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#general-timing-characterisation-parameters)
        + [Timing Threshhold Definitions](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#timing-threshhold-definitions)
        + [Propogation Delay and Transition Time](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/blob/main/DAY%202.md#propogation-delay-and-transition-time)     
* SKY130 DAY 3: Design Library Cell Using Magic Layout and NGSPICE characterisation
     - Labs for CMOS Inverter NGSPICE Simulations
        + IO Placer Revision
        + SPICE Deck Creation For CMOS Inverter
        + SPICE Simulation Lab for CMOS Inverter
        + Switching Threshhold Vm
        + Static and Dynamic Simulation of CMOS Inverter
        + Lab Steps to GitClone VSDSTD Cell Design
     - Inception of Layout Â CMOS Fabrication Process
        + Create Active Regions
        + Formation of N and P well
        + Formation of Gate Terminal
        + Lightly Doped Drain [LDD] Formation
        + Source Â Drain Formation
        + Local Interconnect Formation
        + Higher Level Metal Formation
        + Lab Introduction to SKY130 Basic Layers Layout and LEF using Inverter
        + Lab Steps to Create STD Cell Layout and Extract SPICE Netlist
     - SKY130 Tech File Labs
        + Lab Steps To Create Final SPICE deak using SKY130 tech
        + Lab Steps to Characterise Inverter using SKY130 Model Files
        + Lab Introduction to SKY130 PDKs And Steps to Download Labs
        + Lab Introduction to Magic Tool Options and DRC Rules
        + Lab Introduction to Magic and Steps to Load SKY130 Tech Rules
        + Lab Excercise to Fix **poly.9** error in SKY130 Tech-File
        + Lab Excercise to Implement Poly-Resistor Spacing to Diff and Tap
        + Lab Challenge Excercise To Describe DRC Error as Geometrical Construct
        + Lab Challenge to Find Missing or Incorrect Rules and Fix Them
