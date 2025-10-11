# 🧠 Week 3 Task – Post-Synthesis GLS & STA Fundamentals

## 🎯 Objective

The goal of this week’s task is to:

- Perform **Gate-Level Simulation (GLS)** after synthesis to validate post-synthesis functionality.  
- Understand the fundamentals of **Static Timing Analysis (STA)** and perform basic timing checks using **OpenSTA**.  
- Generate and interpret **timing graphs** to identify setup/hold violations and analyze slack.

---

## 🧩 Part 1 – Post-Synthesis GLS

### 🔍 Objective
To verify that the synthesized design behaves functionally the same as the RTL (Week 2 functional simulation).

### 📚 Reference
- **GLS Reference – VSD_HDP:** [Day 6 – VSD_HDP GitHub Repository](https://github.com/Ananya-KM/VSD_HDP/blob/main/Day%206.md)

### ⚙️ Steps

#### 1. Perform Synthesis
- Run synthesis on your **BabySoC design** using **Yosys**.  
- Generate the **synthesized netlist** and synthesis logs.

#### 2. Run Gate-Level Simulation (GLS)
- Use the synthesized netlist in your simulator (e.g., **Icarus Verilog**).  
- Simulate with the same testbench as your Week 2 RTL simulation.

#### 3. Compare Outputs
- Compare **GLS simulation waveform** with the **RTL functional simulation waveform**.  
- Both results should match if synthesis is successful and functional.

### 📤 Deliverables
- ✅ `synth.log` – Synthesis log files  
- ✅ `gls_waveform.png` – GLS simulation waveform screenshot  
- ✅ `GLS_vs_Functional_Notes.txt` – Short note confirming GLS output = Functional output

---

## 🧮 Part 2 – Fundamentals of STA (Static Timing Analysis)

### 🔍 Objective
To learn and understand the fundamental concepts of **Static Timing Analysis (STA)** such as:

- Setup and Hold checks  
- Slack  
- Clock definitions  
- Path-based analysis  

### 📚 Reference
- **STA Fundamentals – Udemy (Free Course):**  
  [VLSI Academy: STA Checks Course](https://www.udemy.com/course/vlsi-academy-stachecks/?couponCode=F960AEDD365E0CD12546)

### 📝 Deliverable
- ✅ `STA_KeyNotes.pdf` – One-page summary or key notes from the STA course  
  Include:
  - Definition of setup and hold time  
  - Meaning of slack  
  - Role of clock constraints  
  - Interpretation of timing paths  

---

## ⚡ Part 3 – Generate Timing Graphs with OpenSTA

### 🔍 Objective
To perform **Static Timing Analysis** using **OpenSTA** and visualize timing paths and slack.

### 📚 References
- **OpenSTA GitHub Repository:** [The OpenROAD Project – OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA)  
- **Example Script – Day 19:** [VSD_HDP Day 19 Reference](https://github.com/arunkpv/vsd-hdp/blob/main/docs/Day_19.md)  
- **OpenSTA Documentation (PDF):** [OpenSTA Commands and Options](https://github.com/The-OpenROAD-Project/OpenSTA/blob/master/doc/OpenSTA.pdf)

### ⚙️ Steps

#### 1. Load Netlist and Constraints
Launch OpenSTA and load your synthesized netlist and SDC constraints.

```tcl
read_verilog synth_netlist.v
read_liberty my_lib.lib
read_sdc constraints.sdc
link_design top_module

```
### 2. Perform Timing Analysis

Run setup and hold timing checks:
```tcl
report_checks -path_delay max -fields {slew capacitance delay slack}
report_checks -path_delay min -fields {slew capacitance delay slack}
```
### 3. Generate Timing Reports and Graphs

Generate timing report and graph screenshots.

Identify critical paths and note slack values.

### 📤 Deliverables

✅ opensta_script.tcl – OpenSTA TCL input script

✅ timing_report.txt – STA timing report output

✅ timing_graph.png – Timing path/graph screenshot (with userid & timestamp)

✅ Observations.txt – Notes on:

Critical path

Slack meaning and interpretation

### 📅 Summary – By End of Week 3:

✅ Perform Gate-Level Simulation (GLS) and verify post-synthesis correctness.

✅ Understand STA fundamentals (setup/hold, slack, clock, and timing paths).

✅ Use OpenSTA to perform real-world timing analysis and interpret timing reports.

### ✍️ Author

Name: Mohan krishna

Project: Week 3 – Post-Synthesis GLS & STA Fundamentals

Tools Used: Yosys, Icarus Verilog, GTKWave, OpenSTA

