# WEEK3-VSD-RISC-V
VSD RISC-V TAPEOUT PROGRAM WEEK-3 ASSIGNMENT 
# GLS OF BABYSOC
## POST-SYNTHESIS SIMULATION
### Purpose of GLS:
Gate-Level Simulation is used to verify the functionality of a design after the synthesis process. Unlike behavioral or RTL (Register Transfer Level) simulations, which are performed at a higher level of abstraction, GLS works on the netlist generated post-synthesis. This netlist includes the actual gates and connections used to implement the design.

## Key Aspects of GLS for BabySoC:
### Verification with Timing Information:

1.GLS is performed using Standard Delay Format (SDF) files to ensure timing correctness.

2.This checks if the SoC behaves as expected under real-world timing constraints.
### Design Validation Post-Synthesis:

1.Confirms that the design's logical behavior remains correct after mapping it to the gate-level representation.

2.Ensures that the design is free from issues like metastability or glitches.
### Simulation Tools:

1.Tools like Icarus Verilog or a similar simulator can be used for compiling and running the gate-level netlist.

2.Waveforms are typically analyzed using GTKWave.
### Importance for BabySoC:

BabySoC consists of multiple modules like the RISC-V processor, PLL, and DAC. GLS ensures that these modules interact correctly and meet the timing requirements in the synthesized design.

## Here is the step-by-step execution plan for running the commands manually:
### Load the Top-Level Design and Supporting Modules
Launch the yosys synthesis tool from your working directory.
```bash
 read_verilog -sv -I src/include/ -I src/module/ src/module/vsdbabysoc.v src/module/clk_gate.v src/module/rvmyth.v

 read_verilog -I src/include/ src/module/vsdbabysoc.v src/module/rvmyth.v

 read_verilog -I  src/include/  src/module/clk_gate.v

 read_liberty -lib src/lib/avsddac.lib

 read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

 synth -top vsdbabysoc

 read_liberty -lib src/lib/avsdpll.lib

 difflibmap -liberty   src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

 opt

 abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
```
### Result:
<img width="1187" height="677" alt="Result" src="https://github.com/user-attachments/assets/49e0761f-199c-4230-836c-94a30b29b944" />

<img width="1202" height="677" alt="result_2" src="https://github.com/user-attachments/assets/c7060b74-4850-4729-a4d6-c9726576b70d" />


### Perform Final Clean-Up and Renaming:
```bash
yosys> flatten

yosys> setundef -zero

yosys> clean -purge

yosys> rename -enumerate
```
### Result:
<img width="1185" height="682" alt="Perform Final Clean-Up and Renaming" src="https://github.com/user-attachments/assets/6faf53e3-7106-49a9-a2de-4f43a0f261c9" />

<img width="1210" height="676" alt="final-cleanup" src="https://github.com/user-attachments/assets/abbe3f78-c12a-4f1e-8c5e-2fe29b65603f" />



### Write the Synthesized Netlist:

``` write_verilog -noattr output/post_synth_sim output/synth/vsdbabysoc.synth.v```

#POST SYNTHESIS SIMULATION WAVE FORMS:
```bash

 cp -r ~/VSDBabySoC/my_lib/verilog_model/sky130_fd_sc_hd.v .

 cp -r ~/VSDBabySoC/my_lib/verilog_model/primitives.v .

 cp -r ~/VSDBabySoC/output/post_synth_sim/ ~/VSDBabySoC/src/module/

 iverilog -o ../../output/post_synth_sim/post_synth.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY='#1' -I ../include ../../output/synth/vsdbabysoc.synth.v ./sky130_fd_sc_hd.v ./primitives.v ./testbench.v

 ./post_synth_sim.out

 gtkwave post_synth_sim.out
```
### Waveform:
<img width="1279" height="815" alt="Screenshot 2025-10-07 224631" src="https://github.com/user-attachments/assets/ad46e6e7-2f29-4d2a-b55e-f37c93d6c4b7" />

