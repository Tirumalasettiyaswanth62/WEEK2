# WEEK2
VSDBabySoC WEEK2 ASSIGNMENT

# Overview

BabySoC is a simplified, open-source SoC built around the RVMYTH processor core, a RISC-V-based CPU. It integrates a Phase-Locked Loop (PLL) for precise clock generation and a 10-bit Digital-to-Analog Converter (DAC) for interfacing with external analog systems. While the DAC allows BabySoC to interact with devices such as televisions or mobile phones by converting digital signals to analog, this task emphasizes understanding the internal digital behavior through simulation.

# Focus on:
What is a System-on-Chip (SoC)?

Components of a typical SoC (CPU, memory, peripherals, interconnect).

Why BabySoC is a simplified model for learning SoC concepts.

The role of functional modelling before RTL and physical design stages.

# Tools used:
1️. Icarus Verilog (iverilog):

Used for compiling and simulating the BabySoC RTL modules.
Allows verification of functional behavior of the SoC components like CPU, PLL, and DAC.

2️. GTKWave:

Used for visualizing simulation waveforms.
Helps analyze signal interactions, timing, and system behavior in a graphical form.      

# What is a System on a Chip (SoC)?
It is a highly integrated circuit that functions like a mini-computer built onto a single chip. Instead of requiring separate physical parts for each function, an SoC combines all the necessary components into one small package.

used in mobiles etc..

# Introduction to VSDBabySoC
The VSDBabySoC (or BabySoC) is a small yet highly capable SoC with a primary objective of being an educational platform. It was designed to facilitate the simultaneous testing of three specific open-source intellectual property (IP) cores for the first time while also allowing for the calibration of its analog components.

## 1.Initialization and Clock Generation: 
Upon receiving an initial input signal, BabySoC activates the PLL. The PLL generates a stable and synchronized clock signal, which is essential for coordinating the activities of the RVMYTH processor and DAC. By synchronizing the system, the PLL ensures that all components operate in harmony, avoiding timing mismatches and ensuring data integrity.

## 2.Data Processing in RVMYTH:
Within BabySoC, RVMYTH plays a central role in processing data. Specifically, it utilizes its r17 register to hold and cycle through values that are used by the DAC. As RVMYTH executes instructions, it sequentially updates r17 with new data, preparing it for analog conversion. This cyclical processing allows BabySoC to generate continuous data streams that the DAC can output.

## 3.Analog Signal Generation via DAC: 
The DAC receives the processed digital values from RVMYTH and converts them into an analog signal. This output, saved in a file named OUT, can be fed to external devices like TVs and mobile phones, which interpret the analog signals to produce sound or video. This functionality enables BabySoC to interface with consumer electronics, showcasing how digital data can drive multimedia outputs in real-world applications.

# VSDBabySoC Diagram:

<img width="1248" height="698" alt="1" src="https://github.com/user-attachments/assets/b01f1d21-cebf-4b45-9617-ca10769936e0" />
<img width="885" height="535" alt="2" src="https://github.com/user-attachments/assets/71307929-347d-4752-ba96-59a55d888b3d" />
<img width="600" height="556" alt="3" src="https://github.com/user-attachments/assets/f591c33e-64c9-4fba-9b4e-05bb9f102ee4" />

# BabySoC consists of
## The BabySoC integrates both digital and analog parts, including:

RVMYTH Microprocessor (RISC-V CPU): This is the brain of the BabySoC, an open-source, customizable CPU that handles all the processing tasks and communicates with the other parts of the system.

8x Phase-Locked Loop (PLL): The PLL generates a stable and synchronized clock signal to ensure all components—the RVMYTH processor and the DAC—operate in harmony and avoid timing mismatches.

10-bit Digital-to-Analog Converter (DAC): The DAC's function is to receive digital signals from the RVMYTH processor and convert them into an analog output signal.

# VSDBabySoC Simulation & Synthesis Flow
## Project Structure:
This project consists of pre_systhesis_sim result and post_synthesis_sim
results

### 1.PRESYNTHESIS:

1. git clone <repository_url>
You need to replace this with the actual URL of the BabySoC project repository (e.g., a GitHub link).

2.. Compiling and Running the Simulation 
This set of commands navigates into the project directory and uses make to automate the simulation process.

Command:

```bash
## FOR GTKWAVE OUTPUT OF VSDBabySoC:
sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io

sudo chmod 666 /var/run/docker.sock

$ cd VSDBabySoC/

$ python3 -m venv my-venv

$ my-venv/bin/pip install pyyaml click sandpiper-saas

$ source my-venv/bin/activate

$ make pre_synth_sim

$ gtkwave output/pre_synth_sim/pre_synth_sim.vcd

##after this we need to change rvmyth.vls to rvmyth.v for synthesis:

cd ~/VSDBabySoC/

ls VSDBabySoC/
images  LICENSE  Makefile  README.md  src

ls VSDBabySoC/src/module/

avsddac.v  avsdpll.v  clk_gate.v  pseudo_rand_gen.sv  pseudo_rand.sv  rvmyth_gen.v  rvmyth.tlv  rvmyth.v  testbench.rvmyth.post-routing.v  testbench.v  vsdbabysoc.v

# Step 1: Install python3-venv (if not already installed)
sudo apt update
sudo apt install python3-venv python3-pip

# Step 2: Create and activate a virtual environment
cd ~/VLSI/VSDBabySoC/
python3 -m venv sp_env
source sp_env/bin/activate

# Step 3: Install SandPiper-SaaS inside the virtual environment
pip install pyyaml click sandpiper-saas

# Step 4: Convert rvmyth.tlv to Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
If u want the exit virtual environment use "Deactivate" .

## yosys procedure:

yosys> read_verilog -sv -I src/include/ -I src/module/ src/module/vsdbabysoc.v src/module/clk_gate.v src/module/rvmyth.v

yosys> read_liberty -lib /<your_path>/yosys/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -lib src/lib/avsddac.lib

read_liberty -lib src/lib/avsdpll.lib

read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

synth -top vsdbabysoc

write_verilog vsdbabysoc.synth.v

abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 
```
# VSDBabySoC RTL to Gate-Level Simulation Flow:
## 1️.Pre-Synthesis Simulation:
Compile RTL with Icarus Verilog 

Purpose: Verify RTL functionality before synthesis

Output: output/pre_synth_sim/pre_synth_sim.out and waveform .vcd

### Waveform of pre_synthsis_sim:

<img width="1280" height="825" alt="Screenshot 2025-10-04 111447" src="https://github.com/user-attachments/assets/be04cc5e-f242-4579-962d-a31ec543c12e" />

In this waveform, the following signals can be observed:

CLK – The clock input to the RVMYTH core, originally generated by the PLL.

reset – The reset input to the RVMYTH core, coming from an external source.

OUT (real) – A real-valued wire from the DAC module that can simulate analog behavior.

## 2.POSTSYNTHESIS:
Compile synthesized netlist + standard cells with Icarus Verilog

Purpose: Verify gate-level behavior matches RTL

Output: output/post_synth_sim/post_synth_sim.out and waveform .vcd

Here are the command that I have used to get Post synthesis wave form:

```bash
exit yosys
$ cd ~/VSDBabySoC

$ make post_synth_sim.v

$ cd post_synthesis_sim

$$ gtkwave output/post_synth_sim/post_synth_sim.vcd
```
## Here is the waveform of post_systhesis_sim:
![post_sythesis_simulation](https://github.com/user-attachments/assets/3bd5a4e5-86ed-457e-be42-e26f1a26eeb9)

In this waveform, the following signals can be observed:

CLK – The clock input to the RVMYTH core, originally provided by the PLL.

reset – The reset input to the RVMYTH core, coming from an external source.

OUT (wire) – The output of the VSDBabySoC module. This originates from the DAC, but due to simulation limitations, it behaves digitally rather than analog.

D[9:0] – The 10-bit output from RVMYTH register #17, serving as input to the DAC.

OUT (real) – A real-valued wire from the DAC module that can simulate analog behavior.

## 1️.Pre-Synthesis Simulation:

Focuses only on verifying functionality based on the RTL code.

Zero-delay environment, with events occurring on the active clock edge.

## 2️.Post-Synthesis Simulation (GLS):

Uses the synthesized netlist (gate-level) to simulate both functionality and timing.

Identifies timing violations and potential mismatches (e.g., unintended latches).

Helps verify dynamic circuit behavior that static methods may miss.

# Conclusion:
### Understanding Dataflow: 
I was able to visually trace data moving through the SoC. I specifically watched the RV_TO_DAC[9:0] bus to see the 10-bit value that the RISC-V core was sending to the DAC, and then I confirmed how it affected the final OUT signal.

### The Role of Core Signals:
I got a much clearer understanding of how fundamental signals control the system. I saw how asserting the reset signal forced the design into a known initial state, and how the rising edge of the CLK signal drove all the synchronous operations.

### Simulation vs. Synthesis:
A really important lesson was seeing the difference between Verilog for simulation and for synthesis. The DAC model used a real data type to mimic an analog output, which is perfect for simulation. However, I learned that this construct isn't synthesizable, meaning you can't use it to create actual hardware logic, where you'd use a standard wire instead.
