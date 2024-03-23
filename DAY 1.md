# SKY130 Day 1: Inception of OpenSource EDA, OpenLANE and SKY130 PDK

## How to Talk to Computers

### Introduction to QFN - 48 package, chip, pads, core, die and IPs
A commonly and extensively used arduino circuit board can be seen in the below picture. The board has a processer or SoC i.e. System on a Chip. This is a central part of the chip and is encircled as can be seen below: 

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}

![Arduino Board](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ee3cc1ca-c4b2-4879-9310-e6b18eb70959)

The encircled region, however is only a high level view, which can be represented through a block diagram, as shown below:

{IMAGE CREDITS - VSDIAT ; shared as part of lecture and subsequently hand drawn by author}

![Block Diagram](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/df9eced4-0311-45ce-b6a4-ccbd5e04bb25)

In the picture, we are able to see a large box which represents, as written a SoC. In layman's terms, this often called a "CHIP". However, in technicality it is termed as a "PACKAGE". One kind of package, which is used in the arduino board is a QFN - 48 package (Quad Flat No-Leads). A package can be schematically represented as below:

{IMAGE CREDITS - VSDIAT ; shared as part of lecture and subsequently hand drawn by author}

![Chip and Package](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/230f9986-466d-4423-8526-131efee694a0)

As seen, a chip is actually inside a package, and is connected to various "PINS" or inputs/outputs. The locations of the pins and what they are are usually driven by the design of the PCB. A chip is also a very complex system, and has various components such as -: 

* PADS : They act as connectors between the integrated circuit and chip. Pads, simply are structures for sending electrical signals inside the chip. They are strategically placed and made with respect to the pins
* CORE : This is the digital logic (i.e. the various gates such as AND, OR, XOR etc and the MUXes) is placed. It carries out all processor functions.
* DIE : A die is a part of semiconductor wafer that can be used independently in various devices. It can contain more than one core.

The core also has various components, which can mainly be segregated into -:

* FOUNDRY IPs : These are components that are provided to the designer of the SoC by the foundry (i.e. a factory where chips are produced). These components are usually pre-designed and tested by the foundry. Foundry IPs can include PLLs, adc0, adc1, SRAMs and various other components.
* MACROs : Macros are very similar to Foundry IPs, but consist of pure digital logic, that is pre-made for certain common use-cases and can be easily integrated into a SoC or Integrated Circuit (IC). Macros can be of various types such as a GPIO bank and SPI etc.

Two schematics encompassing all of the above components can be seen below:

{IMAGE CREDITS - VSDIAT ; shared as part of lecture and subsequently hand drawn by author}

![schematic 1 - pad, core , die](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9568c3b3-1086-4146-b43a-9768632db86c)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture and subsequently hand drawn by author}

![schematic 2 - macro, foundry ip](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/bb43afab-53f8-4697-96ce-a711ced50c59)

### Introduction to RISC V
RISC-V Instruction Set Architechture, commonly called RISC V ISA, is the language of the computer. When a C program is to be run on a piece of hardware, it is first compiled in an assembly language program like RISC V. It is then converted to machine language i.e. binary and subsequently implemented in the form of a hardware description language such as picorv32 cpu core. A schematic is shown below, representing the same: 

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}

![C program to running on hardware](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/0c10080e-99ac-42f6-8ea9-9f5b5a139c7b)


### From Software Applications to Hardware
Application Software, or apps. They run on hardware such as laptops, mobile phones, tablets etc that are powered by chips. However, apps are coded in complex application programs that require conversion into binary or machine language for running. This is done through system software, which majorly consists of 

* Operating systems - Operating Systems (OSs) perform a variety of functions such as I.O. operation, memory allocation and low level system function. They produce functions in languages such as C, C++, VB and Java, which are sent to the compiler.
* Compilers - Compilers produce simple instructions (the format of which depends on the hardware) in the form of .exe documents, which are to the assemblers. These instructions are abstract interfaces between complex coding languages like C/C++ and hardware, and hence are called ISAs. They are known as the architechture of the computer.
* Assemblers - Assemblers convert the instructions from the compilers into binary, and the function is implemented.

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}

![code to hardware](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/5e3fb422-7ca4-4657-b805-0d314d0deec7)


## SoC Design and OpenLANE

### Introduction to Components of Opensource Digital ASIC Design
ASIC requires mainly three components for design
* RTL IPs - they are building blocks written in a hardware description language. These blocks describe the functioning of the circuit at a basic level and are pre designed and verified.
* EDA Tools - EDA, which expands to Electronic Design Automation tools are used for design, simulation and verification and the analyzing of circuit designs. Common tools are OpenSTA, OpenRoad etc.
* PDK data - Process Design Kit data is a collection of files used to model the fabrication process for EDA tools. It is usually provided by a foundry and include Process Design Rules, Device Models, Digital Standard Cell Libraries and IO libraries.

However, until June of 2020, there was no OPENSOURCE available PDK data, making it impossible to do ASIC design on opensource platforms. Nonetheless, this changed when Google and Skywater released PDK data on 130 nm chips.  There is a unfortunate common misconception that 130 nm is not up to the industrial mark, with chips reaching Armstrong sizes in today's date. This is debunked due to the following two reasons -:
- several applications do not require such advanced nodes, and 130nm chips provide a good amount of power for those cases
- fabrication costs are also much lesser for 130nm chips

130 nm chips are also not slow, as verified by intel and OSU-:

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}

![130nmslow](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ed434895-429d-4b8c-8cb9-5175c4f96165)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/5982d929-56f7-4bf3-b628-2902668be1db) 
### Simplified RTL to GDS flow
The RTL to GDSII  ( Register Transfer Level to Graphic Design System II) design process takes many steps, that are -:

- Synthesis - it converts hardware description languages such as VERILOG into gate-level representations part of a standard cell library. The cells part of this library have a regular layout. Each of these have different views/models such as Electrical, HDL, Spice etc.

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![synthesis](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/119683f4-360c-4164-8944-88b9421de9bb)

- Floor Planning involves optimizing chip performance, area utilization, and connectivity through spatial arrangements (i.e. the layout and placement of various components)The three main purposes of floor planning are firstly, minimizing wire lengths, secondly, reducing signal delays, thirdly, optimizing power distribution, and fourthly, ensuring efficient chip utilization. Power Planning aims to ensure stable and reliable power delivery to all components by effective distribution and design of power supplies and power distribution networks (PDN). The main purposes of this are minimizing voltage drop and noise, reducing power distribution network (PDN) resistances and capacitances, and ensuring uniform power distribution throughout. Usually the chip is powered through VDD pads which are connected to various components through parallel rectangular strips causing lesser resistance.

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/86b639fe-6ad1-470b-8ea0-07919dec535a)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/df18d8bf-407d-4cae-a646-d693c49e3364)

- Placement - it is the process of determing where a component will be placed on the chip. The components can include standard cells, macros, and I/O pads. The cells are usually placed on floorplan rows, and are aligned with the sites. There are majorly two steps - global and detailed.

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/663b03f0-14af-4092-9c41-3b172d41c982)

- Clock Tree Synthesis - This step is done before routing, because the clock needs to be routed by delivering the clock to all sequential elements.

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/21013c30-c388-42f5-b0b4-67212c456aab)

- Routing - The determination of the interconnections of the components throught the various metal layers, whose thickness, pitch etc is detailed by the PDK. The SKY130 has 6 layers.

- Sign Off - this majorly has verification through three processes:
  
  + Design Rule Check (DRC) - this makes sure the design complies with manufacturing guidelines and is compatible for fabrication. It aims to detect and correct layout errors so that fabrication defects do not occur.

  + Layout vs. Schematic (LVS) - here the layout is contrasted against the schematic to ensure consistency. LVS tools extract netlists and compare them for differences, after which the design proceeds to physical design flow

  + Static Timing Analysis (STA) - it evaluates timing behaviour of a digital circuit to ensure design meets setup and hold time constraints, maximum clock frequency, and other timing requirements

### Introduction to OpenLANE and Strive Chipsets
OpenLANE is an open-source digital ASIC jointly developed by efabless and Google, designed to automate the entire design process flow based on several components including OpenROAD, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. It aims to create a clean GDSII (i.e. no LVS violations, no DRC violations, and no timing violations) with no human intervention.  striVe is family of SoCs created by Efabless.

{IMAGE CREDITS:VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/8d894bc9-bc80-45da-a53c-bb476461fd04)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/89bc0308-1952-442b-b446-a853f012eb54)

### Introduction to OpenLANE detailed ASIC Design Flow

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/3f05412f-4afe-4abd-9d0f-2587877af2d9)

- Synthesis Exploration - it generates a delay vs area report

  {IMAGE CREDITS - VSDIAT ; shared as part of lecture}
  ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/4708be8d-0ab3-4d56-94b7-ddb1b89e0fea)

- Design Exploration - sweeps design configuration and subsequently find best configuration for any given design. It produces a report as shown:

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
  ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/0f3818e5-3ab5-4a5e-b7ac-2eb56bab1d30)

- OpenLANE Regression Testing

 {IMAGE CREDITS - VSDIAT ; shared as part of lecture}
 ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e08e166f-bb34-488f-8bfb-a69e8ef1634d)

- Design for Test (also k/a DFT)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/428bec23-7853-48f5-b64c-f8b3dae21c63)

- Physical verification (DRC & LVS) - Magic is used for DRC and Magic and Netgen for LVS.

- Logic Equivalence Check (LEC) - checks that the physical implementation and the netlist have the same logic. It is performed each time netlist is modified and checks that changing netlist did not change function.

- Dealing with Antenna Rules violations

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9a1efcf8-2a59-4037-a5a4-c7e67ba0bed2)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/3341e402-c8eb-4d8e-a602-ee5232fb0231)

{IMAGE CREDITS - VSDIAT ; shared as part of lecture}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/287c01c9-a8bf-47af-8ff7-21094a704407)

- Timing Analysis (STA) - Here, the input also contains a synthesized netlist along with other data.

## Get Familiar to Opensource EDA tools

### OpenLANE Directory Structure in Detail
Openlane is more of a flow than a tool comprising of the various opensource EDA tools such as Yosys, OpenSTA. The aim of openlane is to automate the entire RTL to GSII flow and make it clean and opensource. It is very similar to commercial EDA tools.

Exploring OpenSource directory through Linux terminal steps:-

1) Open the virtual machine (made as instructed through the document Kunal sir shared) and then open terminal on it.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/63dd9800-4e1c-424f-b2e9-44dca5126a88)

2) Type _cd Desktop_ and then _cd work/tools_ to change directory to Desktop/work/tools, as this is where all openlane files are stored.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1a317af8-a4eb-41fe-b308-45307fb47a49)


3) After changing directory, type _ls -ltr_ to list the contents of the directory.

> Side note: **ltr** represents how the list of contents should be ordered . To find other ways, type _ls --help_.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/6a6f1f08-cb16-4e44-937a-9a0a0eaf288a)

4) In this workshop, we are going to use **openlane_working_dir**, and hence we will change directory to it by typing _cd openlane_working_dir_. Afterwards, we list the contents using the same _ls -ltr_. In this workshop, we are going to use **pdks** and **openlane** directories and hence we will explore both in sequence.

5) Starting with **pdks**, type _cd pdks_ and then _ls -ltr_ to view the contents in **pdks**. Here, we will explore **sky130A**, so similarly change directory and then view contents. One will observe two directories - _libs.tech_ and _libs.rif_

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/3679332d-0999-406a-a40c-97e1829053dd)

6) _libs.tech_ contains all tool specific files. As seen in the picture - tools like Qflow, netgen, magic etc have directories. Opening the **magic** directory in **libs.tech**, we can see the following files:

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/b41bee95-a59e-4cc8-8094-85c53c2ce138)

7) _libs.ref_ contains all the technology/foundry related processes. Upon further exploration of **sky130_fd_sc_hd**, we see the following

> **_cd .._** reverses the directory one step behind to the parent directory.cd 

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/963b1a8e-6b7d-4078-845a-f61abe497e5e)

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f01b000a-5b3e-49d3-a6f2-28e116f8fa02)

8) Next, we will open **openlane**

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/75bc59d6-41af-4854-8fd1-9bd58dd8616d)c

### Design Preparation Step

To open Openlane, we can use the _docker_ command using **interactive**. After invoking the docker command, the prompt changes to **bash-4.2$**, and then one must type _ls -lrth_, and subsequently _./flow.tcl -interactive_ _package require openlane 0.9_ retrives all the required information for openlane.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/13868bec-d11f-4b63-aae4-1fd3191adfbb)

OpenLane has various designs and we are most interested in **picorv32a** Inside the _designs_ folder there is document named _config.tcl_ which overrides the default settings. These configurations are design specific.(e.g. clock period, clock port, verilog files).

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/455bdb9f-366d-431b-b3c9-e47b39ea9091)

To setup the design of OpenLANE, we first need to prepare the design through _prep -design picorv32a_, which gives the following result.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/6fe84e83-52d9-43fa-a135-2fa39e90ffef)

The command when run, sets up a filesystem where the OpenLANE can store the results. This creates a folder inside the **picorv32a** directory which contains the command log files, results, and the reports dumped of the various tool. The folder will be only have the lef files generated by this design setup stage. The cell LEF files *.lef* and technology LEF files *.tlef* merge to generate merged.lef inside **runs/tmp/**, wherein a a folder with today's date will be created, inside which a **tmp** folder will have contents, and the _merged.lef_ folder will contain the merged _lef_ files.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f273ec37-1225-41ee-b3af-bacdb26df037)

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e7e38963-ce9b-403c-a483-0a43f94c7276)

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/6ab6ea83-5f09-4601-b1ee-38fdf4c1cec8)


PROGRESS SO FAR:

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/82a332a5-3be0-4dc1-aa0e-3b2b7698cd8c)

### Review Files After Design Prep and Run Synthesis

Opening the **merged.lef** file through the _less_ command after design prep will give one a document as shown:

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/fa5c1059-c09e-4bf2-ade1-7cd0adaa1bac)

After this, type the command _run_synthesis_ to run synthesis to run yosys RTL synthesis, ABC scripts (for technology mapping) and openSTA. This process will take about 5-6 minutes, depending on system speed.

### Steps to Charecterise Synthesis Results

After synthesis, under point 20, there are printing statistics, which can be used to calculate flip-flop ratio. 

> FLIP FLOP RATIO = NO OF DFFs / NO OF CELLS * 100

Solving this through our data, we get => 1613/18036*100 = 8.94%

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/afd10f2b-4199-44ed-9a1b-6d56fe71cb7c)

After this, one may look at the results of the synthesis through hte **picorv32a.synthesis.v** file, which can be found as shown, and then viewed through the _less_ command.

{IMAGE CREDITS: AUTHOR ; screenshot taken from device}  
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/4c82387b-9c37-4187-b689-0ec19812aaf4)

Also, to look at the clock timing report, we can go into **reports** folder by _cd_ command, and then view the report named **1-yosys_4.stat.rpt**, or the STA report named **2-opensta.timing.rpt** through the _less_ command, which can be exited by pressing the _Q_ key.




