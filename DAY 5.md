# SKY130 DAY 5: Final Steps For RTL2GDS Using TritonRoute and OpenSTA
## Routing and Design Check [DRC]
### Introduction to Maze Routing and Lee's Algorithm

Routing is to find the best possible connection [route] between two elements [clocks, flip-flops etc]. There have been many routing algorithms created like Steiner Tree algorithm, Line Search algorithm etc. and one such algorithm we will go into detail is **Maze Routing** - Lee's Algorithm (Lee 1961). 

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/df16571e-0a37-413b-a7e6-a3646b122aee)

Consider connecting points 1 and 2 in the above picture, where in 1 is the source and 2 the target. The need is to find the best possible [shortest] path to connect 1 and 2. This route will aim to have very little or none routes with zig-zags, and mostly the routes are L-shaped. From an algorithmic standpoint, the software needs to search and connect the two poi+nts. From a physical design point of view, it is a physical/wire that allows signals to travel.

Lee's Algorithm is popularly used in maze routing [type of problem where path from source to destination needs to found in grid similar to a maze]. This algorithm is especially effective for routing in grid or mesh-based structures, making it an excellent choice for integrated circuit design.

Steps:

1. Initialisation - In this step, a routing grid/matrix is created in the area to be routed. It categorises each cell into one of various states - (i) obstacle ; (ii) empty ; (iii) visited ; (iv) source ; (v) target.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/9893e362-d22c-4a50-bd05-8429647033f8)

2. Wave Expansion - The algorithm performs a wave expansion from the source [marked as 'S'] cell, spreading outwards in all directions. At each step, the algorithm examines neighboring cells (up, down, left, and right) and assigns them a value one greater than the minimum value of their neighboring cells (excluding obstacles and starts from 1). This process continues until the target [marked as T] cell is reached or until no more cells can be visited.
   
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/4c3d992d-6bde-4e43-a901-b0bddafa0e4b)

### Lee's Algorithm Conclusion

[Continuation of Steps]

3. Backtracking and Path Reconstruction - Once the target is reached, the algorithm backtracks the path to the source by following the cell values. Through this there might be multiple paths obtained but the tool will choose one with less bends i.e. a shorter route along the algorithm to give us the shortest route.

> The route should not be diagonal and must not overlap any blockage/obstruction such as macros or HIPs.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/d81589f1-dc79-4d3d-9fab-83f4a369fa19)

### Design Rule Check

When we route, we do not just merely connect two points, we must also adhere to certain rules. For example, one rule mention how when constructing two wires there must be a minimum distance between them, they must have a minimum width and pitch etc. ence, DRC cleaning is done to ensure that the routes can be fabricated and printed in silicon properly.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/0ca79794-eec7-446b-baba-1bef47a13257)

Another critical issue is **signal short**, which causes functionality failure. This can be removed by moving the route to the next layer. This leads to more DRCs (via width, via spacing, higher metal layer must be wider than lower metal layer etc.).

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/b8098b78-4113-4c5c-8ab7-cc9cf474cd2b)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/87f5217b-15d8-47a7-aacc-d101f92cb03c)
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/c62c4681-7b0a-4801-905b-f0d413b6eb19)

## Power Distribution Network and Routing
### Lab Steps To Build Power Distribution Network

The lab steps to build a power distribution network are -:

1. Go to the openlane directory

2. Enter the command _docker_ and then **./flow.tcl -interactive** . To subsequently get the openlane package, type _package require openlane 0.9_.
   
3. Then prep the design using -: **prep -design picorv32a -tag [folder name of run where in cts had been done]
  
4. Then type **echo $::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/[folder name of run where in cts had been done]/results/cts/picorv32a.cts.def**
   
5. Then, type this to generate the PDN -: **gen_pdn**

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/45346852-d350-4783-b997-4bd00b010117)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/78e82b6b-b037-4a99-a746-69400ef7da2d)

### Lab Steps From Power Straps To STD Cell Power

The power and ground rails have a pitch of 2.72um and hence the custom inverter cell also has a height of 2.72um. This is as otherwise power and ground rails will not be able to power the cell. We also see that looking at the LEF file runs/[date]/tmp/merged.lef, all cells are have height of 2.72um and only their width differs.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1927eb7a-660a-41b4-8e95-2169ff9ce5dd)

It is shown below how standard cells are powered up :- power/ground pads -> power/ground ring-> power/ground straps -> power/ground rails

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ff3e440b-1e96-4602-9f9d-4a4fdb8375c4)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/538c64bd-f71a-4d1a-bcde-b5901ba7f521)

### Basics of Global and Detail Routing and Configure TritonRoute

**TritonRoute** is the engine that is used for routing through the **run_routing** command. 

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/8178e934-0397-4e74-82c0-68990b24606f)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/2521622c-dfb7-4f33-84de-cbf6ecabfac7)

In the VLSI flow, the routing stage is highly critical and can be executed using either open-source or commercial tools. This stage is divided into two phases:

Global Route / Fast Route:

This is accomplished using fast routing techniques where the area to be routed is partitioned into tiles or rectangles. Global routing establishes the initial framework for routing paths.
Detail Route:

This phase involves meticulous tracking routing techniques to complete the routing process. Detailed routing fine-tunes and finalizes the paths to ensure proper connectivity and compliance with design constraints.

In VLSI, routing is extremely important and is either done through open-source/commericial tools. It has mainly two phases -:
- Global Route - Also known as fast route, it uses fast routing techniques wherein we partition the area to routed into tiles/rectangles. It establishes the initial framework
- Detailed Route - This process uses more meticulous routing techniques, and completes the process by fine-tuning and finalizing the paths to ensure connectivity and compliance with DRC.

## TritonRoute Features
### TritonRoute Feature 1 - Honors Pre-processed Route Guides

The preferred direction for M1 and M2 respectively is vertical and horizontal. Whenever, a non-preferred direction route is found by the tool, the tool divides the route into unit width in a process called splitting. The sections that fall into the preferred directions are subsequently combined. Sections parallel to the preferred direction are bridged with upper layer [in bridging]. Non-preferred routes are converted into preferred routing guides of M2.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/f86b25d9-5bfc-42d9-a9a9-134a888d5c5c)


### TritonRoute Feature 2 & 3 - Inter-Guide Connectivity and Intra & Inter Layer Routing

The second feature is interguide connectivity:-
![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/884ff7fc-f58a-407f-99e4-f51ed7ffcf2a)

As mentioned earlier, the preferred direction of M1 is vertical - resulting in vertically oriented lines. Panels are the dashed lines with a routing guide for each panel. Intra layer panel routing is when routing occurs withing even-index panels. Initially, routing takes place simultaneously in all even-index panels, which is followed by routing in odd-index panels. This routing remains confined within a particular layer and then Routing progresses from lower to upper layers, ensuring the orderly flow of routing operations.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/4a811495-b77f-4892-a0a3-6811f8f06111)

### TritonRoute Method To Handle Connectivity

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/3374eb33-3680-4ce4-a86a-bd278db2f5e6)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/7b41590c-4d09-49a8-9885-d5284c174357)

The aim of MILP (Mixed Integer Linear Programming) is to find the optimal solution to connect two access point clusters. 

In the below algorithm, cost is found for each access point. Then, a minimum spanning tree between access points and cost is made. So, the algorithm says that the minimal and most optimal point is needed between two APCs.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/ec94132d-4372-4355-a70a-0b68bd3491f8)

### Routing Topology Algorithm and Final Files List Post Route

After running the _run_routing_ command, routing [both global and detail] is completed. Routing Strategy was set to 0, which meant that DRC violations must be brought down from a high value [25000 in this case] to 0. This takes multiple iterations [34] and nearly 20 minutes to half an hour.

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/1dc01a94-ef0e-4422-bed4-e439f05f9990)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/b35b31de-c361-4a56-83a1-df61382580e4)

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/84ebc449-4c0a-4fa0-9361-79ab24272988)

After this, a def file will be formed in the location [runs/[date]/results/routing/picorv32.def], which has to opened in MAGIC -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/5b519467-4617-43c8-837f-d7074d29c1f0)

##### Parasitic Extraction

> NOTE: OpenLANE does not have any spef extraction tool, so we use a separate tool present in work/tools/ directory.

The steps for extraction are -:
1. Go to /home/vsduser/Desktop/work/tools/SPEF_EXTRACTOR, where in there are a list of files, one of which is a python file called main.py. This file helps in generation of the SPEF provided. There are also lef & def files in the folder.
   
2. Now, to create the SPEF file **python3 main.py /home/vsduser/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_05-49/tmp/merged.lef  /home/vsduser/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_05-49/tmp/routing/picorv32a.def**

> NOTE: spef will be saved in the same location as def file. /home/vsduser/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_05-49/tmp/routing

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/b9e3dbe7-3ff4-468f-8099-cd3762d94261)

The last stage will be to extract the GDSII file ready for fabrication run_magic

This uses Magic to stream the GDSII file runs/26-03_05-49/results/magic/picorv32a.gds. This GDSII file can then be read by Magic:


The last stage is to extract the GDSII file ready for fabrication through **run_magic**. This uses MAGIC to stream the GDSII file **runs/26-03_05-49/results/magic/picorv32a.gds**. The GDSII can now be ready by MAGIC -:

![image](https://github.com/ojasvi-shah/Advanced-Physical-Design-Using-OpenLANE--Ojasvi-Shah/assets/163879237/232ff315-d75d-45c4-865c-a6e6d8c9685a)
