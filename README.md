# VSD-PD-workshop

# Contents
<a href="#day-1-understanding-of-riscv-based-picosoc"> 1. Understanding RISCV ISA, Qflow and QFN package </a>

<a href="#day-2-introduction-to-floorplanning-and-placement"> 2. Introduction to floorplanning and placement</a>

<a href="#day3-concepts-of-spice-simulations-layout-and-fabrication-steps"> 3. Concepts of spice simulations, layout and fabrication steps</a>

<a href="#day-4-Timing-analysis-and-clock-tree-synthesis"> 4. Timing analysis and clock tree synthesis</a>

<a href="#day-5-Routig-DRC-and-the-concept-of-SPEF"> 5. Routing, DRC and the concept of SPEF</a>

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
# Lab: 
In linux terminal, copied an existing rtl from vsdflow github folder in a local directory called my_picorv32, and prepared to run synthesis using osu018 technology. 

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

# Lab: 

![Image of Yaktocat](https://github.com/dipta30/VSD-PD-workshop/blob/main/images1/8.png)

# Day 3: Concepts of spice simulations, layout and fabrication steps

•	Writing spice deck for inverter in ngspice, static (dc) simulation

•	Significance of switching threshold (point where Vin = Vout) in robustness of the inverter

•	Dynamic simulation (inverter is excited with a pulse input)

•	Importance of Euler path and stick diagram together in making neat layouts with lesser connectivity

•	Layout of inverter in magic

•	post-layout simulation in ngspice

•	Comparison of pre- and post-layout plots

•	16-mask CMOS fab process: detailed description

# Day 4: Timing analysis and clock tree synthesis

## Timing analysis: 
Setup and hold analysis with ideal clock vs. with real clock

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

# Day 5: Routing, DRC and the concept of SPEF

•	Understanding Maze routing and Lee’s algorithm to find the shortest and best routing path

•	DRC: design rule check 

•	SPEF

# Labs:
For routing and post-route STA, type

cd vsdflow/my_picorv32

qflow route picorv32

qflow sta picorv32

qflow backanno picorv32

leafpad log/sta.log

Routing going on is shown below



