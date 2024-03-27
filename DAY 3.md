# SKY130 DAY 3: Design Library Cell Using Magic Layout and NGSPICE characterisation
## Labs for CMOS Inverter NGSPICE Simulations
### IO Placer Revision

OpenLANE configurations can be changed inside the shell itself, on the fly. IO Mode is usually set to _random equidistant_. However, if we want to change this, we can do so through the following command typed after floorplan : **set ::env(FP_IO_MODE) 2**. After running this command, the IO [input - output] pins will not be equidistant in mode 2 (instead of the default - that is 1). 

After this, we may re-run floorplan, and then check by seeing that the pins are placed based on of Hungarian algorithms now i.e. stacked one over the other. 

> NOTE: changing the configuration on the fly will not change the runs/config.tcl, the configuration will only be available on the current session.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/64fccafc-180a-4e66-881a-80fabe083dc1)

### SPICE Deck Creation For CMOS Inverter

The SPICE deck contains connectivity information about netlists, inputs to be provided, TAPS for the outputs etc. The component values are taken, that are usually -: for the PMOS it is .375u/.25u (i.e. the channel length is .25 micron and and the channel width is .375 micron). Ideally, the PMOS should be 2 to 3 times wider than the NMOS. This is as the PMOS hole carrier is slower than the NMOS carrier, and since the rise and fall time must be matched, to reduce the resistance, we increase the width of the PMOS. The next steps are to identify and name the nodes:

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e85697ff-266b-4fc5-83a9-c3fe0143ffcf)

The syntax of the SPICE deck netlist PMOS and NMOS is _[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]_. It is to be noted that all components in a netlist are described based on its node and values.

> EXAMPLE SYNTAX - M1 OUT IN VDD VDD PMOS W=.375U L=.25u

### SPICE Simulation Lab for CMOS Inverter

The start of SPICE simulation is _.op_ where in Vin will be swept from 0 to 2.5 with 0.05V steps. The model file is **tsmc_025um_model.mod** that has all the technological parameters for the 0.25um NMOS and PMOS.

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/bdec7a54-4667-4c1f-acbc-193062f2bcda).

For SPICE simulation, there are various steps-:

1) Open the NGSPICE simulator
2) Source the Circuit File through _source_ command
3) Execute it by the command _run_ and then use _setplot_ which allows one to view any plots possible from the simulations specified in the spice deck and will give you a choice for which simulation to be run
4) Then, type _display_ which will give you a choice of nodes to be plotted which when _plot out vs in_ is typed will be plotted on a graph.

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/a4931bc2-cb5d-4e9e-8c14-54bc916c0b00)

### Switching Threshhold Vm

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/da38d8fa-1309-485b-b68d-e0e35c819a0a)

POINTS TO BE NOTED:

- The shapes of the graphs are almost the same, through which we can derive the conclusion that CMOS is a robust device
- The parameters that determine the robustness of the CMOS is the switching threshhold and the propogation delay

The Switching Threshhold is the point where the the input voltage is equal to the output voltage and both PMOS & NMOS are in saturation region. When these are turned on, there is a high chances of leakage and  that the current flows directly from VDD to GND. Due to this, short circuit can be seen.

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/07f581e6-f0c1-49b3-b190-18c4d5a05157)

### Static and Dynamic Simulation of CMOS Inverter

To find Vm, we use DC TRANSFER ANALYSIS. Simulation is essentially a sweep from 0V to 2.5V by taking 0.05V steps.

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/7709cad2-6227-4878-baad-d3165745ef67)

To find propogation delay, we use transient analysis when a pulse is applied to the CMOS.

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1797489d-2861-4149-879b-496c616550db)

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9dda14bc-11c3-4469-9d27-4ad812765df2)

### Lab Steps to GitClone VSDSTD Cell Design

We have been provided with a github repository wherein inverter files lie. It is available at this link - https://github.com/nickson-jose/vsdstdcelldesign. Steps to clone and observe the layout are as follows:
1. Clone the custom inverter standard cell design from the github repository shared above
2. Clone the repository with the custom inverter design through the command _git clone https://github.com/nickson-jose/vsdstdcelldesign_
3. Subsequently, copy the tech file to the _vsdstdcelldesign_ directory (created through above step) by this command _cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/_
4. Then, open the custom inverter layout in MAGIC through this command: _magic -T sky130A.tech sky130_inv.mag &_cp__

{IMAGE CREDITS: AUTHOR ; SCREENSHOT TAKEN FROM DEVICE}
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

{IMAGE CREDITS: VSDIAT ; SCREENSHOT TAKEN FROM LECTURE}
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
     + Titanium Diselenide [Ti2Si2] is used for local interconnects
     + Mask 11 is formed and Titanium Nitride [Ti N] is etched off by RCA cleaning to create the first level contact

### Higher Level Metal Formation

8. Higher Level Metal Formation - These steps are very similar to the previous steps and are quite easy to understand.
     + The previous steps in the MASK process have created an uneven surface layer. A layer of Silicon Dioxide [SiO2] doped with phosphorous or boron -[boron reduces the temperature] [known as phosphosilicate glass and borophosphosilicate glass] is deposited on the wafer surface.
     + Then, the surface is polished using the CMP [Chemical Mechanical Polishing] technique to planarize the surface.
     + Contact holes are created through photolithography.
     + Various masks are used for the various processes after this:-
          - Mask 12 is created for the first contact holes
          - Mask 13 is used for the first Aluminum contact layer, which the contact holes are connected to.
          - Mask 14 creates the second contact holes
          - Mask 15 is similarly, for the second Aluminum contact layer
          - Finally, we use Mask 16 for making contact to topmost layer

### Lab Introduction to SKY130 Basic Layers Layout and LEF using Inverter

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/c4eb61e9-abe9-40fb-8191-0a032c8cd91c)

> In sky130A, the first layer is the LDD or local-i and then the m1, m2 layers and so on.
> The power and ground lines are located in m1.
> When polysilicon crosses ndiffusion then NMOS and if polysilicon crosses pdiffusion then PMOS is created.
> The output of the layout is the _LEF_ file, which is used by the router in APR to get the location of standard cell pins for proper routing. So it is essentially an abstract form of the layout of a standard cell.

### Lab Steps to Create STD Cell Layout and Extract SPICE Netlist

For the SPICE extraction of the custom inverter layout, we can enter the following commands in the TCKON -:
1. _**extract all**_
2. _**ext2spice cthresh 0 rthresh 0**_ --> This extracts the parasitic information
3. _**ext2spice**_

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/225ebb8d-8128-425c-99f9-5531a119e69d)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f8b6fbce-cd2a-40d4-bb31-856cb7ad54d1)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/2344ca21-bbad-4c56-8cba-3fd709f749e8)

## SKY130 Tech File Labs
### Lab Steps To Create Final SPICE deck using SKY130 tech

The default SPICE deck file using Sky130 is seen in the previous section 👆. Now, we can modify the file to plot a transient response, which would then create a final SPICE deck file by editing as below.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e1536061-189f-4851-a5c5-7651c55cc75e)

To load the SPICE file for simulation in NGSPICE, type the following command : _**ngspice sky130A_inv.spice**_

### Lab Steps to Characterise Inverter using SKY130 Model Files

After typing this command, you will get a result as follows:-

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/eb4bf7a0-c3aa-4cd3-b619-6ee5cf335397)

After this, to generate a graph, type the command _**plot y vs time a**_ to generate a plotted graph of the transient analysis through NGSPICE -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e3e5f0ea-a497-4107-a3f8-efeec27c5424)

Using this, we can characterise the cell through four parameters - 
1. value of rise transition, which is the time taken for an output waveform to transit from a value of 20% of the maximum value to 80% of the maximum value = vdd = 3.3V. 20% of 3.3V is about .66V, and 80% is about 2.64V, and hence we will click on those points, whose x and y values will appear in the terminal as seen -:

   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/a8ef6eaa-d887-4eb8-8d7a-749eab8b8699)

   Then, rise transition = 2.2457ns - 2.1819ns = 0.638
   
2. value of fall transition, which is the time taken for an output waveform to transit from a value of 80% to 20% of the maximum value. Similarly, 4.06818ns - 4.04073ns = 0.02745ns

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9a48997f-5cc4-4295-9000-b161eea44a86)


3. value of fall cell delay, which is the time difference {delay} between 50% of the input and 50% of the output which essentially means the time taken for output to fall to 50% and time taken for input to rise to 50%. Calculating fall delay => 4.05402ns - 4.0501ns = 0.00392ns

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ff01b40d-6c29-4298-a644-dad980ebf0f4)


4. value of rise cell delay, which is the time difference {delay} between 50% of the input and 50% of the output which essentially means the time taken for output to rise to 50% and time taken for input to fall to 50%. Calculating rise delay => 2.18381ns - 2.15003ns = 0.03378ns

   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/b91f4493-9d17-42dc-aa97-4bce99645185)


Through this, we have charecterised a cell at 27 degrees Celsius successfully -:
[cell was plotted with analysis occuring at 27 degrees Celsius]
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/29ceff2d-b483-4a24-b0e1-1ce0e4de0588)

### Lab Introduction to Magic Tool Options and DRC Rules

A documentation shared by the instructor on how MAGIC performs DRC [Design Rule Check] and it's syntax for the rules is at [this link](http://opencircuitdesign.com/magic/) 

### Lab Introduction to SKY130 PDKs And Steps to Download Labs
Now, for the lab we need to download the lab files, which can be done through this command -: _**wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz**_

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/c0814fc7-b71f-4410-a4e6-00983bddbe9b)

Then, to extract these files, we can type _**tar xfz drc_tests.tgz**_

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/0cd7d114-5975-4882-aa0a-7410ef8cbed8)

Then we will go into the directory by **_cd drc_tests_** and then list the contents through _ls -al_. Then to open the magic tool, type _**magic -d XR**_

### Lab Introduction to Magic and Steps to Load SKY130 Tech Rules

Then, in the empty magic prompt, click **"FILE"**, which is at the top and select _open_ and then _met3.mag_ under that, to obtain this screen view wherein we can see a number of independent layouts contain certain DRC errors

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/812972b8-a90a-4924-b762-200269bf94b3)
![modified](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/d0d3fdb3-17f0-405e-b3d4-c022dce1895b)

> NOTE: the text in purple is the periphery rules broken

### Lab Excercise to Fix **poly.9** error in SKY130 Tech-File
### Lab Excercise to Implement Poly-Resistor Spacing to Diff and Tap
