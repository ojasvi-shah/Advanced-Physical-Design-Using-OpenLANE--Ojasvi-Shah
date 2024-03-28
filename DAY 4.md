# SKY130 DAY 4 : Pre-Layout Timing Analysis and Importance of Good Clock Tree
## Timing Modelling Using Delay Tables
### Lab Steps To Convert Grid Information Into Track Information

_sky130_inv.mag_ located in _vsdstdcelldesign_ directory contains all information like PG and port information, logic etc. OpenLANE is a PnR tool and hence does not require the full information present in the _.mag_ file. The only information that we require are the boundary, power and ground rails, and the inputs & outputs information. This is the reason of we use **.lef** files. Hence, our next  objective is to extract the LEF file from the MAGIC file and plug that into the picorv32a design. The guidelines to be followed while making a standard cell are -:

1. The input and output ports must lie at the intersection of the horizontal and vertical tracks (this is to ensure the routes can reach the ports).
2. The width and height of the standard cell must be odd multiples of the track's horizontal and vertical pitch respectively

> Tracks refer to the horizontal and vertical metal layers on which routing occurs. The grid formed by the intersection of horizontal and vertical tracks creates a routing grid, also k/a a routing matrix.

The ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info contains track information. Before and After changing the grid values -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/aea3baeb-f06a-4fc1-bc49-403f5fc576dc)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/389e6a93-2f1f-485b-ba46-e035bb3b896e)

Hence, we are able to satisfy the first guideline. 
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/db9b4d6c-7eeb-4fa8-8291-3d72f300c712)

Then, we are able to satisfy the second guideline as follows -:
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1f114768-5c36-459e-9d8f-0bb8895912d8)

### Lab Steps to Convert Magic Layout to STD Cell LEF

LEF [Library Exchange Format] which contains information of the standard cell library used in the design. They contain detailed information about the geometric shapes, sizes, layers, and other physical properties of individual cells or macros within the library. The instructions to set the port definitions are available [here](https://github.com/nickson-jose/vsdstdcelldesign#create-port-definition). Next, save the _.mag_ file with a new filename by typing _**lef write**_ in the tkcon terminal, which will generate a new **lef** file with the new filename -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/60742f8a-ba20-474b-892e-10989f16efa8)

### Introduction of Timing **libs** and Steps To Include New Cell in Synthesis

Inside this directory [_pdks/sky130A/libs.ref/sky130_fd_sc_hd/lib/_] are the liberty timing files for SKY130 PDK which have the timing and power parameters for each cell needed in STA. It can either be slow, typical, fast with different supply voltages (1v80, 1v65, 1v95), which are called PVT corners. The library named **sky130_fd_sc_hd__ss_025C_1v80** describes the PVT corner as slow-slow [ie. delay is maximum], 25° Celsius temperature, at 1.8V power supply. Timing and power parameter of a cell are obtained by simulating the cell in a variety of operating conditions [i.e. different corners] and this data is represented in the liberty file, which characterizes all cells and is used during ABC mapping during synthesis stage which maps the generic cells to the actual standard cells available in the liberty file.

- Firstly, copy the extracted lef file - named _**sky130_vsdinv.lef**_ and the liberty files named _**sky130*.lib**_ from this repository - _**/openlane/vsdstdcelldesign/libs**_ to the **src** directory of **picorv32a**.

  ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9cdf6857-1eb6-4d9a-954d-83d46acf3548)

- Subsequently, add the following to the _**config.tcl**_ file inside the _picorv32a_ directory. Through this, we are setting the liberty file that will be used for ABC mapping of synthesis (LIB_SYNTH) and for STA (_FASTEST,_SLOWEST,_TYPICAL) and also the extra LEF files (EXTRA_LEFS) for the customized inverter cell.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/849e78c9-01b5-462b-81a2-e43d8f260172)

-   After this, invoke the _**docker**_ command and prepare the _**picorv32a**_ design. Plug the new _lef_ file to the OpenLANE flow through the following commands

> docker
>
> 
> ./flow.tcl -interactive
>
> 
> package require openlane 0.9
>
> 
> prep -design picorv32a
>
> 
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
>
> 
> add_lefs -src $lefs
>
> 
> This will generate the following picture -:
>
> 
> ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/2de2bc50-1ee4-4f11-bbfd-05f34ac555d2)

- Then, run synthesis using the _run_synthesis_ command and check that sky130_vsdinv cell is successfully included in the design
  ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/138f4c76-f7e8-477e-9258-bfc344a37432)

> NOTE : here, timing is not fixed, which then needs to be fixed.
>
> 
> ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/83438d2c-b993-460f-a4f9-f8c9d55f2e4c)

### Delay Tables

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/17a6e767-5d3e-418b-82f6-bd46df41d0cc)

The above image depicts to us how the enable pin affects the CLK propogation. In case of AND gate, ONLY when the enable pin is equal to 1, the CLK propogates to Y. Similarly, in case of OR gate, the CLK propogates to Y only when the enable pin is 0. Whenever the enable pin is equal to 1, the CLK does not propogate and there is no short circuit power consumption. Switching power consumption when such elemements are used is used in CLOCK TREE. This method is known as Clock Gating Technique.

However, when we think of this, the question that arises is _**HOW DOES ONE USE THIS?**_ For this purpose, consider the below clock tree structure -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/db284022-8ee5-4145-ba0d-920fdfd1de59)

We see through the above picture that buffers on different levels have different capacitive loads and buffer sizes but as long as they have the same loads and sizes in the same level, the total delay for each clock tree path will be the same and, so skew will remain zero. However, we can observe that practically, different levels can have varying input transition and output capacitive load and hence varying delay.

We use delay tables to observe each cell's timing models that is included inside the _.lef_ files. The element that majorly impacts delay is _output slew_, which depends on capacitative load and the input slew [which is a function of previous stage buffer's output capacitive load and input slew and has its own transition delay table.]

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/27168e84-7202-48bb-9e87-9ca3e0131c91)

We notice at level 2 that both buffers have identical delays due to equal transition times, load capacitances and buffers sizes. Subsequently, we see that the _skew_ is zero. However if this is not true, the skew will be negative, which will cause timing violations. On a small scale, these are often considered to be insignificant but we see their importance in large designs which millions of cells, where in if we fail to adhere to guidelines during clock tree creation, many timing-related complications can be caused.

> CTS is the process of designing a clock distribution network to minimize skew and ensure synchronous operation of the circuit
> 
> Skew refers to the variation in clock signal arrival times, and slew [rate] is the rate of change of signal's voltage on time
> 
> Latency is the delay experienced by the clock signal

### Lab Steps To Configure Synthesis Settings to Fix Slack and Include VSDINV

We can see through previous pictures that currently, 
- tns = -711.59
- wns = -23.89
- chip area for the module picorv32a = 147712.9184

Hence, our next step is to try to make the synthesis more **timing driven**. This can be done as follows-:

1. Firstly, check synthesis strategy and other timing related variables and then modify accordingly
    - SYNTH_STRATEGY of delay 0 will mean that the tool will focus more on optimizing the delay
    -  index can changed to be 0, 1, 2, or 3 where 3 is the most optimized for timing at the cost of area.
    -  SYNTH_BUFFERING of 1 will ensure that the buffer will be used on high fanout cells to reduce wire delay.
    -  SYNTH_SIZING of 1 will enable cell sizing where cell will be upsized or downsized as needed to meet timing.
    -  SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength.
  
      ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1acfa7d0-6ec9-4570-bf98-bbb3826f3776)

2. After this, run synthesis again. It is will be seen that area is increased but there is no negative slack
     + tns = 0
     + wns = 0
     + Chip area for module picorv32a = 209181.872

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/7d194ab7-bf43-438e-91a0-725386bd61dc)

3. Subsequently, we may run floorplan and placement

> NOTE : If any error comes related to macro placement, temporary solution is to comment **basic_macro_placement** inside the file **OpenLane/scripts/tcl_commands/floorplan.tcl** (this is okay since we are not adding any macro to the design).
>
> We can also execute the following commands:-
> 
> init_floorplan
> 
> place_io
>
> global_placement_or
>
> detailed_placement
>
> tap_decap_or
>
> detailed_placement

After successful run, **runs/[date]/results/placement/picorv32a.placement.def** will be created:-

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/fad383de-088c-49c9-8571-5ba57734cab3)

4. After this, search for instance of cell **sky130_vsdinv** inside the DEF file after the placement stage through this command **cat picorv32a.placement.def | grep sky130_vsdinv**

5. Then, select a single **sky130_vsdinv** cell instance from the list dumped by grep (e.g. 41096). On tkcon, command **select cell 41096** may be run. Press ctrl+z to zoom into that cell. As shown below, our customized inverter cell sky130_myinverter is sucessfully placed. Use the **expand** command on tkcon to show the footprint of the cell and notice how the power and ground of sky130_vsdinv overlaps the power and ground pins of its adjacent cells -:

   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/eec0ea29-808b-4356-bce0-d8543bc17310)
   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/4440d05f-d511-4fd9-bf4c-f15b111b069e)
   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/700908e3-8609-4d52-94f6-6d7eb1f386b1)

## Timing Analysis With Ideal Clocks Using Open STA
### Setup Timing Analysis And Introduction to Flip-Flop Setup Time

Consider an ideal clock [i.e. a clock tree is not built yet] where perform timing analysis to understand the parameters [later the same can be done using real clocks]. Specifications are as mentioned in the picture [Clock frequncy (F) is 1GHz and clock period (T) is 1ns]. -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/39437ad9-3953-4955-ac5f-e15c60e3adf0)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f12cdcd3-e080-4d25-977c-aed998a2e060)

The setup timing analysis equation is = Θ < T - S

> Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop
> 
> T = Time period, also called the required time
> 
> S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/0f2910d0-0c34-40f1-aaf5-40c311efe669)

### Introduction to Clock Jitter and Uncertainty

CLK signals are sent to the device by the PLL (Phase Locked Loop). This clock source is expected to send clock signal at 0, T, 2T etc. However, even these clock sources might or might not be able to provide a clock **exactly** at Tns because of its own in-built variation known as jitter. Jitter can be thought of as short-term fluctuations in the timing of signal transitions, which can result in deviations from the expected clock or data timing.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/a298949c-3101-44c3-b15a-d60efbc9eca8)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ca549e3e-4895-4a62-b3e6-4924e2394d2d)

Hence, we see that a more realistic equation for setup time is = Θ < T - S - SU

> SU = Setup uncertainty due to jitter which is temporary variation of clock period, which is due to non-idealities of PLL.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/48e91fce-8564-4295-9b94-4b50afa9288f)

### Lab Steps to Configure OpenSTA for Post-Synth Timings Analysis

STA can either be -:
+ single corner - which only uses the LIB_TYPICAL library which is the one used in pre-layout(pos-synthesis) STA
+ multi corner - multicorner which uses LIB_SLOWEST(setup analysis, high temp low voltage),LIB_FASTEST(hold analysis, low temp high voltage), and LIB_TYPICAL libraries

1. This is the configuration file on which we'll be doing pre layout analysis -:

   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/d38bb100-ed9c-43ed-9ac1-c6d3c4869f90)

2. This is the SDC file

   ![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ac226b7b-d347-4feb-8b53-2895d0ff049e)

3. The various commands to be run and their functions are -:

   + _create_clock_ -it creates clock for the port with specified time period.
     
   + *set_input_delay* and *set_output_delay* - defines the arrival/exit time of an input/output signal relative to the input clock. [This is the delay of the signal coming from an external block and internal delay of the signal to be propagated to external ports] This adds a delay of Xns relative to clk to all signals going to input ports, and delay of Yns relative to CLK to all signals going to output ports.
     
   + *set_max_fanout* - specifies maximum fanout count for all output ports in the design.
     
   + *set_driving_cell* - models an external driver at the input port of the current design.
     
   + *set_load* sets a capacitive load to all output ports.

4. Execute *sta pre_sta.conf* and check timing.

### Lab Steps to Optimize Synthesis to Reduce Setup Violations
### Lab Steps to do Basic Timing ECO

## Clock Tree Synthesis TritonCTS and Signal Integrity
### Clock Tree Routing and Buffering Using H-Tree Algorithm

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/b7ebc401-975d-4c82-94cb-980d7c724bb0)

Take into account the CLK port going to the various flip-flops in the above picture. It's purpose is to connect the port to the CLK pins of the many flip-flops based on the connectivity information given. In the above picture, we have blindly made connections and hence t2 is greater than t1. Skew is the difference between t2 and t1. CLK skew, as explained earlier, refers to the variation in arrival times of the clock signal at different points within a digital system. More simply, CLK skew is essentially the difference in propogation delay of the CLK signal as it moves along various paths. It can occur due to -:
- differences in wire lengths
- variations in signal routing paths
- variations in buffer delays
- other physical and environmental factors

Due to this, some parts of the system can recieve CLK signals earlier than or after the rest of the system. Minimizing clock skew is essential as to -:
- ensure proper synchronization of signals
- reliable operation of the digital circuit.

> NOTE : Ideally, the skew should be zero.

Now, since the skew is not minimum, or even close to minimum in the above picture, we can call this tree a **_bad tree_**. To optimize this scenario, we can use H-Tree routing. It analyses the clock route by calculating the distance from the source to all the endpoints and deciding on a midpoint to start building tree. Subsequently, it finds another midpoint and creates divergents in the wire. Eventually, after repeating this process, the CLK reaches all the flip-flops at almost the same time, while splitting up at various midpoints.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/0fdaf27e-01a3-4ba6-b32b-5bc4d4a6beaf)

It is expected that the input CLK signal is exactly reproduced at the output. However, this does not happen in H-Tree routing due to inherent resistance and capacitance in physical wires, which may cause the signal to experience distortion. We can solve this problem by inserting buffers/repeaters along the path for signal integrity. In the past, we have also talked about another kind of repeaters, used in data paths. The key differences between repeaters used in clock and data paths respectively lie in their **rise and fall times**. Clock buffers have **same rise and fall times**, to ensure uniform signal propagation throughout the clock distribution network. Contrastingly, data buffers exhibit **varying rise and fall times**, which may differ based on the characteristics of the data being transmitted and the components involved in processing it.

### Crosswalk and Clock Net Shielding

Clock nets are critical nets in the design because clock tree is built is such a fashion that the skew is zero. There is a phenomenon called crosstalk where a signal transmitted on one channel  interacts with or interferes with signals on adjacent channels leading to distortion, noise, timing errors etc, which can cause the clock tree structure will be deteriorated. To solve this, all the clock nets are shielded [protected]. If there is a wire adjacent to such shields, then there exists a huge coupling capacitance causing two issues - (i) glitch and (ii) delta delay.

Whenever there is a switching activity happening in the aggressor, then the coupling capacitance is so strong that it directly affects the net close to it named _the victim net_, which is without any shielding. This causes a dip in the voltage, resulting in glitch. Due to a glitch, there can be incorrect data in memory, which can cause inaccurate functionality. To solve this, we do shielding which protects the victim nets by breaking the coupling capacitance between the aggressor and the victim. These shielding nets are either in the form of Vdd or Vss. The shields do not switch, and hence the victim will not switch as well.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/e19160f6-cd94-4572-9470-336db0ca6a7a)

### Lab Steps to run CTS using TritonCTS
### Lab Steps to Verify CTS Runs
## Timing Analysis  With Real Clocks Using Open STA
### Setup Timing Analysis Using Real Clocks

Now the clock tree is built and timing analysis is done on real clocks.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/cc3c96ca-817f-4559-aefa-baa6a3583b78)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/bdd0b465-92b3-4d34-b389-eb0545d5048e)

> delta1 = launch flop clock network delay
>
> 
> delta2 = capture flop clock delay

Any design satisfying Slack [i.e. Data required time - Data arrival time] is ready to work in the given frequency. However, if this equation is violated, then slack will become negative which is not expected. We expect slack to be either zero or positive.

### Hold Timing Analysis Using Real Clocks

Hold Time is delay [time] needed by the MUX2 model within a flip-flop to transfer certain data outside. [i.e. how long it **holds** the data]. It is the time period during which the launch flop must retain dat before it reaches the capture flop. Unlike setup analysis, which has two rising clock edges, hold analysis occurs on the same rising clock edge for both the launch and capture flops. 

A hold violation occurs when the path is too fast, impacted by factors such as -:
 - combinational delay
 - clock buffer delays
 - hold time.
 
 > Notably, parameters such as time period and setup uncertainty hold no significance, as both launch and capture flops receive identical rising clock edges during hold analysis.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/367dc2a4-b701-4ef6-9cf0-a6e2048b6564)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/3ed9941f-37df-4ec4-8dc7-28191831cdc1)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/5ad6901b-57b8-4d7a-a8f1-9215f70ca7d9)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/82b6ac76-15a9-4a0d-b496-76e1a9d3b012)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f781227a-ddff-4d2a-8878-c54bb95127c6)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/72ae228c-3900-494f-93ea-ffbd30880ad9)


### Lab Steps to Analyse Timing With Real Clocks Using OpenSTA
### Lab Steps to Excecute OpenSTA With Right Timing Libraries and CTS Assignment
### Lab Steps to Observe Impact of Bigger CTS Buffers On Setup And Hold Timing
