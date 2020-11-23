# VSD-PD-workshop

# Contents
<a href="#day-1-review-of-riscv-based-picosoc">Review of RISCV based picoSOC</a>

<a href="#day-2-introduction-to-floorplanning-and-libraries"> Introduction to floorplanning and libraries</a>

# Day 1: Review of RISCV based picoSOC
•	Understanding the computer system (where instruction set architecture comes in play) 

•	The instruction-set architecture (ISA) depends on the type of CPU core. RISCV CPU core will need RISCV based IS format.

•	RISCV is open source assembly language program.

•	RISCV implemented on picorv32 CPU core. RTL to GDS is done in Qflow.

•	Qflow is a digital synthesis flow tool chain using open-souurce EDA tools

•	Open source tools: Logic synthesis (Yosys), floorplanning, placement, CTS (Graywolf), routing (Qrouter), Static timing analysis (opentimer), pre-layout and post-layout spice simulations – ngspice, layout (magic)

•	Using vsdflow as a wrapper around qflow to get all open-source tools installed.

•	The CPU core will be inside a SOC consisting of some other blocks like memory etc. The chip will consist of the SOC and some additional blocks.

•	Introduction to QFN- 48 package, pad frame (core and die), inside chip : Foundry IP’s (ADC, PLL, SRAM etc. ) and macros (pure digital logic)

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
