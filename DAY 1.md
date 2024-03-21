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
### Introduction to OpenLANE and Strive Chipsets
### Introduction to OpenLANE detailed ASIC Design Flow

## Get Familiar to Opensource EDA tools

### OpenLANE Directory Structure in Detail
### Design Preparation Step
### Review Files After Design Prep and Run Synthesis
### Steps to Charecterise Synthesis Results





