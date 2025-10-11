# Timing Graphs using openSTA
## Introduction
Static Timing Analysis is one of the many techniques available to verify the timing of a digital design.

An alternate approach used to verify the timing is the timing simulation which can verify the functionality as well as the timing of the design.

The term timing analysis is used to refer to either of these two methods - static timing analysis, or the timing simulation.

Thus, timing analysis simply refers to the analysis of the design for timing issues.

The STA is static since the analysis of the design is carried out statically and does not depend upon the data values being applied at the input pins.

This is in contrast to simulation based timing analysis where a stimulus is applied on input signals, resulting behavior is observed and verified, then time is advanced with new input stimulus applied, and the new behavior is observed and verified and so on.

# Installation of OpenSTA
1.clone repository
```
    git clone https://github.com/parallaxsw/OpenSTA.git
    cd OpenSTA
```
2.Build the Docker Image
```
sudo docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```
3.Run the docker image
```
sudo docker run -i -v $HOME:/data opensta
```
# Timing Analysis Using Inline Commands
Once inside the OpenSTA shell (% prompt), you can perform a basic static timing analysis using the following inline commands:

```
# Instructs OpenSTA to read and load the Liberty file "nangate45_slow.lib.gz".
read_liberty /OpenSTA/examples/nangate45_slow.lib.gz

# Intructs OpenSTA to read and load the Verilog file (gate level verilog netlist) "example1.v"
read_verilog /OpenSTA/examples/example1.v

# Using "top," which stands for the main module, links the Verilog code with the Liberty timing cells.
link_design top

# Create a 10ns clock named 'clk' for clk1, clk2, and clk3 inputs 
create_clock -name clk -period 10 {clk1 clk2 clk3}

# Set 0ns input delay for inputs in1 and in2 relative to clock 'clk'
set_input_delay -clock clk 0 {in1 in2}

# Report of the timing checks for the design 
report_checks
```
<img width="1191" height="785" alt="Timing Analysis Using Inline Commands" src="https://github.com/user-attachments/assets/23672517-c5b7-42cb-b41e-f08c97d47c71" />

This represents a setup (max delay) corner, so the analysis focuses on setup timing by default.

How to Also Get Hold (min) Paths:

If you want both setup and hold timing checks (i.e., both max and min path delays), use:
```
report_checks -path_delay min
```
Here are the commands for Yosys synthesis for example1.v:
```
cd ~/VSDBabySoC/OpenSTA/examples/

yosys

read_liberty -lib nangate45_slow.lib

read_verilog example1.v

synth -top top

show
```

Below is the netlist diagram generated using Yosys.

The datapath has been annotated with delay values at each stage for easier understanding:

<img width="1307" height="808" alt="week3 1 2" src="https://github.com/user-attachments/assets/65d75614-384f-4dfa-96fa-4c58141694d3" />

### SPEF-Based Timing Analysis
Here’s the same OpenSTA timing analysis flow with added SPEF-based parasitic modeling:

This enables more realistic delay and slack computation by including post-layout RC data, improving timing signoff precision.
```read_liberty /OpenSTA/examples/nangate45_slow.lib.gz
read_verilog /OpenSTA/examples/example1.v
link_design top
read_spef /OpenSTA/examples/example1.dspef
create_clock -name clk -period 10 {clk1 clk2 clk3}
set_input_delay -clock clk 0 {in1 in2}
report_checks
```
<img width="1121" height="777" alt="SPEF-Based Timing Analysis" src="https://github.com/user-attachments/assets/d5a3037f-768d-4cfc-988a-6403f8727c6d" />


### Timing Analysis Using a TCL Script
To automate the timing flow, you can write the commands into a .tcl script and execute it from the OpenSTA shell.
min_max_delays.tcl
```
# Load liberty files for max and min analysis
read_liberty -max /data/VSDBabySoC/OpenSTA/examples/nangate45_slow.lib.gz
read_liberty -min /data/VSDBabySoC/OpenSTA/examples/nangate45_fast.lib.gz

# Read the gate-level Verilog netlist
read_verilog /data/VSDBabySoC/OpenSTA/examples/example1.v

# Link the top-level design
link_design top

# Define clocks and input delays
create_clock -name clk -period 10 {clk1 clk2 clk3}
set_input_delay -clock clk 0 {in1 in2}

# Generate a full min/max timing report
report_checks -path_delay min_max
```


