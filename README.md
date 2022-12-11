# OpenFASoC - Fully Open-Source Autonomous SoC Synthesis using Customizable Cell-Based Synthesizable Analog Circuits

OpenFASoC is a project focused on automated analog generation from user specification to GDSII with fully open-sourced tools.This whole flow is to automate the gigantic time taking process of designing Analog systems, which are often used in chips and SoCs. It is led by a team of researchers at the University of Michigan and is inspired from FASoC which sits on proprietary software.

More About FASoC: https://fasoc.engin.umich.edu/

More About OpenFASoC: https://openfasoc.readthedocs.io/en/latest/

----------------------------------------------------------------------------------------------
# PHASE LOCK LOOP (PLL) Generator

Phase Lock Loop (PLL) is a system that consists of three major parts; Phase Frequency Detector (PFD), Charge Pump and Loop Filter, and Voltage Controlled Oscillator (VCO). A PLL is highly preferred because it is a feedback system that compares the output frequency from the input frequency and can survive in a single chip. A PLL is normally used in well-timed clock generator, recovery of signal from noisy communication channel and high performance wireless with additional application in PLL’s parts

![PLL](https://user-images.githubusercontent.com/110079763/206902456-795412e8-caba-44ee-948b-121798dc1cb1.png)


## Charge Pump

The charge pump circuit is connected with loop filter and located within PFD and VCO. Charge pump is functioning as a converter for the logic states of the PFD into an analog signal in order to control the VCO. The frequency of the VCO is controlled by the output signal of the charge pump circuit. The output voltage of the charge pump circuit must be held at a constant voltage, when PLL locks in some frequency. The charge pump consists of two switched current source that pump charge in or out of the
loop filter according to two logical inputs.

![CP](https://user-images.githubusercontent.com/110079763/206902465-51f3c27a-219a-4a0c-b768-72321d4f9ed4.png)

# Phase frequency detector
![PFD](https://user-images.githubusercontent.com/110079763/206902472-fdc42b56-e77f-4ea3-8196-719e90638ee0.png)

# Frequency Divider
![FDD](https://user-images.githubusercontent.com/110079763/206902481-f9512a87-907c-4027-8454-f614a1c85504.png)

# VCO
![VCO](https://user-images.githubusercontent.com/110079763/206902490-9021933d-ac0f-4eaf-bcc7-464abacefee9.png)


# MY TASK: Generating AUX CELLS for OpenFASoC Flow
Aux Cells are part of big analog design which cannot be implemented with exisiting standerd cell in library. For different designs we have different aux cells. We have to manually create these.

# AUX CELL GENERATION FOR - OpenFASoC(Fully Open-Source Autonomous SoC)


## GENERATING .lef, .gds for Aux cells

**Discription** : In Open FASoC Flow to generate a automated Analog design, few auxilaury cells(.lef,.gds) are required to be created which cannot be implemented with existing library cells (like Header and SLC in temp_sence_gen). To generate these .lef and .gds files of AUX cells we use ALIGN.

### Reduired inputs from previous step of flow:
 - SCHEMATIC and SPECIFICATION of AUX cell to be generated.
(usually AUX cell contains 6 to 12 transistors)

### First Step 

- Depending upon given SCHEMATIC and SPECIFICATION of AUX cell, a SPICE Netlist will be created with .sp file extension.

### Using ALIGN: Analog Layout, Intelligently Generated from Netlists:

**About:**

ALIGN is an open source automatic layout generator for analog circuits jointly developed under the DARPA IDEA program by the University of Minnesota, Texas A&M University, and Intel Corporation.

The goal of ALIGN (Analog Layout, Intelligently Generated from Netlists) is to automatically translate an unannotated (or partially annotated) SPICE netlist of an analog circuit to a GDSII layout. The repository also releases a set of analog circuit designs.

The ALIGN flow includes the following steps:

Circuit annotation creates a multilevel hierarchical representation of the input netlist. This representation is used to implement the circuit layout in using a hierarchical manner.
Design rule abstraction creates a compact JSON-format represetation of the design rules in a PDK. This repository provides a mock PDK based on a FinFET technology (where the parameters are based on published data). These design rules are used to guide the layout and ensure DRC-correctness.
Primitive cell generation works with primitives, i.e., blocks at the lowest level of design hierarchy, and generates their layouts. Primitives typically contain a small number of transistor structures (each of which may be implemented using multiple fins and/or fingers). A parameterized instance of a primitive is automatically translated to a GDSII layout in this step.
Placement and routing performs block assembly of the hierarchical blocks in the netlist and routes connections between these blocks, while obeying a set of analog layout constraints. At the end of this step, the translation of the input SPICE netlist to a GDSII layout is complete.

#### Installing ALIGN:
**Prerequisites**

- gcc >= 6.1.0 (For C++14 support)
- python >= 3.7 

Use the following commands to install ALIGN tool.

```
export CC=/usr/bin/gcc
export CXX=/usr/bin/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public

#Create a Python virtualenv
python -m venv general
source general/bin/activate
python -m pip install pip --upgrade

# Install ALIGN as a USER
pip install -v .

# Install ALIGN as a DEVELOPER
pip install -e .

pip install setuptools wheel pybind11 scikit-build cmake ninja
pip install -v -e .[test] --no-build-isolation
pip install -v --no-build-isolation -e . --no-deps --install-option='-DBUILD_TESTING=ON'
```

#### Making ALIGN Portable to Sky130 tehnology

Clone the following Repository inside ALIGN-public directory

```
git clone https://github.com/ALIGN-analoglayout/ALIGN-pdk-sky130
```

move `SKY130_PDK` folder to `/home/ritesh/Documents/GitHub/OpenFASoC/AUXCELL/ALIGN-public/pdks`

#### Running ALIGN TOOL

Everytime we start running tool in new terminal run following commands.

```
python -m venv general
source general/bin/activate
```
Commands to run ALIGN (goto ALIGN-public directory)


```
mkdir work
cd work
```
General syntax to give inputs
```
schematic2layout.py <NETLIST_DIR> -p <PDK_DIR> -c
```

#### FLOW

Creating a Python virtualenv
![PYTHON](https://user-images.githubusercontent.com/110079763/206902533-2dc7c419-86ac-437d-b154-55b74f0c2764.png)

Running design
![ALIGN1](https://user-images.githubusercontent.com/110079763/206902537-b4ba353a-4604-4b2e-a3be-0f8d9638c7a2.png)
![ALIGN2](https://user-images.githubusercontent.com/110079763/206902541-b9fa1995-dd98-4cbc-aa1e-8f313ea0d0a5.png)

#### Generated .lef and .gds

# GDS

![GDS](https://user-images.githubusercontent.com/110079763/206902674-74e3c306-c16d-4ed5-9f42-9ec61c75d609.png)

# LEF
![LEF](https://user-images.githubusercontent.com/110079763/206902686-155a089e-d283-4bbb-9230-e02317618c4b.png)

# FUTURE WORK

1. Pre-layout simulation is not matching with Post-layout simulation.  
2. ALIGN PDK are been used in the design, this encounters an issue at OpenFasoc.

## Acknowledgement
  
  * Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.
  * Madhav Rao, Professor, IIIT-Bangalore.
  * Nanditha Rao, Professor, IIIT-Bangalore.
  * Vasanthi D R, PhD Scholar, International Institute of Information Technology, Bangalore vasanthidr11@gmail.com

## Contact Information

  *Ritesh Lalwani, Post Graduate student, IIIT Bangalore, ritesh.lalwani@iiitb.ac.in
  * Kunal Ghosh, Director, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com
  * Madhav Rao, Professor, IIIT-Bangalore. mr@iiitb.ac.in
  * Nanditha Rao, Professor, IIIT-Bangalore. nanditha.rao@iiitb.ac.in
  

# REFERENCE 

- Lakshmi S, MS ECE - MAIL: lakshmi.sathi96@gmail.com GITHUB: https://github.com/lakshmi-sathi/avsdpll_1v8
