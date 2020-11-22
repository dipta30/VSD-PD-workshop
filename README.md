# VSD-PD-workshop

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

