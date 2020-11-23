# VSD-PD-workshop

# Contents
<a href="#day-1-understanding-of-riscv-based-picosoc"> 1. Understanding RISCV ISA, Qflow and QFN package </a>

<a href="#day-2-introduction-to-floorplanning-and-placement"> 2. Introduction to floorplanning and placement</a>

<a href="#day3-concepts-of-spice-simulations-layout-and-fabrication-steps"> 3. Concepts of spice simulations, layout and fabrication steps</a>

<a href="#day-4-Timing-analysis-and-clock-tree-synthesis"> 4. Timing analysis and clock tree synthesis</a>

<a href="#day-5-routing-drc-and-the-concept-of-spef"> 5. Routing, DRC and the concept of SPEF</a>

# Day 1: Understanding RISCV ISA, Qflow and QFN package 
•	Understanding the computer system (where instruction set architecture comes in play) 

•	The instruction-set architecture (ISA) depends on the type of CPU core. RISCV CPU core will need RISCV based IS format.

![1](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/1.png)

•	RISCV is open source assembly language program.

•	RISCV implemented on picorv32 CPU core. RTL to GDS is done in Qflow.

•	Qflow is a digital synthesis flow tool chain using open-souurce EDA tools

•	Open source tools: Logic synthesis (Yosys), floorplanning, placement, CTS (Graywolf), routing (Qrouter), Static timing analysis (opentimer), pre-layout and post-layout spice simulations – ngspice, layout (magic)

•	Using vsdflow as a wrapper around qflow to get all open-source tools installed.

•	The CPU core will be inside a SOC consisting of some other blocks like memory etc. The chip will consist of the SOC and some additional blocks

•	Introduction to QFN- 48 package, pad frame (core and die), inside chip : Foundry IP’s (ADC, PLL, SRAM etc. ) and macros (pure digital logic)

![2](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/2.png)
# Lab session: 
In linux terminal, copied an existing rtl from vsdflow github folder in a local directory called my_picorv32, and prepared to run synthesis using osu018 technology.
```
git clone https://github.com/kunalg123/vsdflow.git
```
![3](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/3.png)

type

```
cd

cd vsdflow

mkdir my_picorv32

cd my_picorv32

mkdir source synthesis layout

cp ~/vsdflow/verilog/picorv32.v source/.

qflow gui &

```

![4](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/4.png)

then put parameters:

Technology = osu018

Verilog source file : picorv32.v

Verilog module : picorv32

Then run preparation

![5](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/5.png)


# Day 2: Introduction to floorplanning and libraries

## Floorplanning

•	Importance of utilization factor and aspect ratio

•	Pre-placed cells: complex combinational logic IP’s (memory, complex clock gating cell, mux, comparator etc) are implemented once and instantiated multiple times in the netlist. User-defined location. placed before automated PnR.

•	Floorplanning: the arrangement of these pre-placed cells in the chip

•	Decoupling caps: surround pre-placed cells with decaps to prevent voltage going out of noise margin region

•	Power planning (VDD and GND grids): done to avoid ground bounce and voltage droop

## Placement
•	Library is for keeping Standard cells (FF, buffers), with different functionality, sizes, different threshold voltage

•	Binding netlist to physical cells from library

•	Initial placement based on input-output connections

•	Optimised placement by calculating estimated wire delay and capacitance and inserting repeaters (buffers)

•	Timing threshold: for a rising waveform we have definitions of slew_low_rise_threshold (typ. 20 %) and slew_high_rise_threshold (80 %) etc.  For input rising wave we have in_rise_thr (50 %)

•	Rise delay = Difference between time-axis coordinates of the 50% point of output and input waveforms

# Lab session: 

For synthesis and placement, continue the steps from day 1. When the qflow gui opens, and the pre-synthesis preparations are done, in the synthesis option, click run.

![6](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/6.png)

Put,

Initial density = 0.7 (typically chosen)

Aspect ratio =1 (for square shape)

Click set stop

![7](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/7.png)

Arrange pins -> auto group -> apply.

![8](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/8.png)

Now, click on 

Arrange pins -> new group.

A new group will be created (here, dipta_grouping). Drag and drop desired pins in this group. Uncheck top, left and right to keep bottom selected, so that the pins placed in this group are placed at the bottom side of the chip.

![9](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/9.png)

Apply. Run placement.

![10](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/10.png)

After placement, can check the log file by clicking OK. 

![11](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/11.png)

To check the size of the chip layout, go to terminal again,
```
cd

cd vsdflow/my_picorv32

qflow display picorv32 &
```
This will open layout and tkcon window in the layout window, select whole chip. Now in tkcon window, type below command
```
box
```
The area will be displayed

![12](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/12.png)

# Day 3: Concepts of spice simulations, layout and fabrication steps

•	Writing spice deck for inverter in ngspice, static (dc) simulation

•	Significance of switching threshold (point where Vin = Vout) in robustness of the inverter

•	Dynamic simulation (inverter is excited with a pulse input)

•	Importance of Euler path and stick diagram together in making neat layouts with lesser connectivity

•	Layout of inverter in magic

•	post-layout simulation in ngspice

•	Comparison of pre- and post-layout plots

•	16-mask CMOS fab process: detailed description

# Lab session:

To run dc simulations for an inverter, type
```
cd

git clone https://github.com/kunalg123/ngspice_labs

cd ngspice_labs

cat inv.spice
```
the spice netlist is displayed on terminal

![13](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/13.png)

To run ngspice for this deck, type
```
ngspice inv.spice
```
A ngspice terminal opens, type
```
run

setplot dc1

plot out
```
This will generate the voltage transfer characteristic (VTC).

For transient analysis, type
```
leafpad inv_tran.spice
```
![14](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/14.png)

This will open up the spice deck. Type
```
ngspice inv_tran.spice

run

setplot tran1

plot out in
```

![15](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/15.png)

![16](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/16.png)

the rise delay can be calculated by clicking on the points where the two input and output plots are 50 % amplitude as shown below. Here the rise delay is ~60 ps. 

![17](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/17.png)

To open the inv layout in magic, type
```
magic -T filename.tech
```
The layout opens. In the tkcon window type
```
source_draw_fn.tcl
```
the layout completes (some interconnects are added in the layout)

![20](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/20.png)

Type
```
magic -T filename.tech fn_postlayout.mag &
```
in thw tkcon window, type
```
extract all

ext2spice
```

![21](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/21.png)

A spice file created from post layout which contains parasitics, opens up.

![22](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/22.png)

Add input excitation parameters and library model files to this and run a transient simulation on this. The output is shown below.

![23](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/23.png)


# Day 4: Timing analysis and clock tree synthesis

## Timing analysis: 
• Setup and hold analysis with ideal clock 
• CTS
• Setup and hold analysis with real clock


## CTS: 

•	Clock is routed using H-tree algo for having zero skew

•	Path is long, contain RC parasitics, clock waveform degrades.  Therefore buffers are introduced to strengthen the signal

•	Clock nets are shielded from crosstalk (problems like glitch) using vdd and gnd routes surrounding the clock nets because these do not switch

•	Glitch – aggressor going from logic 1 to 0, victim was in logic 1 , may go to logic 0 for some time before coming back to logic 1 again (a dip in voltage, ma cause reset in some logics)

•	Delta delay- even after building a Clock tree with zero skew, crosstalk can lead to a delta delay, i.e. a skew. (due to a aggressor going from 0 to 1, victim trying to go from 1 to 0 may tend to go to 1 then comes to 0 after a delay)

•	To have zero skew, Buffers at each stage should be identical and should be driving identical load caps

•	To calculate latency from and to different clock ports, buffer delays need to be calculated 

•	To charcterize a buffer, there are delay tables and transition tables

•	Delay table is output load cap vs input transition time.

# Lab session:

To run STA, type

```
cd

cd vsdflow/my_picorv32

leafpad picorv32.sdc
```
In the file picorv32.sdc file type
```
create_clock -name clk -period 2.5 -waveform {0 1.25} [get_ports clk]
```
Save and close. 
```
leafpad prelayout_sta.conf
```
Type below lines in prelayout_sta.conf file which you have just opened above

```
read_liberty /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib

read_verilog synthesis/picorv32.rtlnopwr.v

link_design picorv32

read_sdc picorv32.sdc

report_checks
```
This script reads the .lib, .sdc and .v file and links the top module to STA tool for analysis.

![24](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/24.png)

Save and close. Type command
```
sta prelayout_sta.conf
```
observe the SLACK value

![25](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/25.png)

see below 'sta' terminal like below
```
%
```
Type 
```
report_checks -digits 4
```
This gives the reports upto 4 digits of precision. Observe the data arrival time and data required time.


![26](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/26.png)


![27](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/27.png)


# Day 5: Routing, DRC and the concept of SPEF

•	Understanding Maze routing and Lee’s algorithm to find the shortest and best routing path

•	DRC: design rule check 

• Each wire has parasitic resistance and capacitancce, which need to be extracted and fed to the tool.

•	SPEF : standard parasitic exchange format

# Lab session:
For routing and post-route STA, type

```
cd vsdflow/my_picorv32

qflow route picorv32

qflow sta picorv32

qflow backanno picorv32

leafpad log/sta.log
```
Routing going on is shown below


![28](https://github.com/dipta30/VSD-PD-workshop/blob/main/images_all/28.png)

# Acknowledgement

Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)

