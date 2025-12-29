# FPGA-Based ECG Signal Denoising using IIR Filters

![Verilog](https://img.shields.io/badge/Language-Verilog-blue?style=flat-square&logo=verilog)
![MATLAB](https://img.shields.io/badge/Tool-MATLAB%20%26%20Simulink-orange?style=flat-square&logo=mathworks)
![Vivado](https://img.shields.io/badge/Hardware-Xilinx%20Vivado-red?style=flat-square)

## 📖 Project Overview
This project focuses on the design and hardware implementation of a cascaded **Infinite Impulse Response (IIR)** filter system to process and denoise raw Electrocardiogram (ECG) signals. The primary objective is to remove common noise sources such as **Baseline Wander**, **High-frequency interference**, and **Power Line Noise (50Hz)** to extract a clean QRS complex for analysis.

## ⚙️ Filter Specifications & Features
The system utilizes a combination of **Elliptic** and **Notch** filters to achieve optimal noise suppression with minimal filter order, ensuring efficient resource usage on FPGA.

### 1. High Pass Filter (HPF)
* **Filter Type:** Elliptic IIR.
* **Purpose:** Removes low-frequency artifacts, specifically **Baseline Wander** (0.5 Hz cutoff).
* **Characteristics:** Chosen for its sharp transition band to effectively block DC offset without attenuating the P-wave.

### 2. Low Pass Filter (LPF)
* **Filter Type:** Elliptic IIR.
* **Purpose:** Eliminates high-frequency noise, such as **Electromyographic (EMG)** interference (muscle noise) and sensor artifacts.
* **Characteristics:** Provides a steep rolloff to cut off high frequencies while maintaining a lower filter order compared to Butterworth/Chebyshev.

### 3. Notch Filter
* **Filter Type:** IIR Notch.
* **Center Frequency:** 50 Hz.
* **Purpose:** Specifically designed to reject **Power Line Interference (PLI)** at 50 Hz.

## 💻 Technologies & Workflow
The project follows a **Model-Based Design** workflow:

1.  **Algorithmic Design (MATLAB):**
    * Designed Elliptic and Notch filter coefficients in **MATLAB** to meet spectral requirements.
    * Analyzed frequency response (Magnitude/Phase) and Pole-Zero plots.
2.  **Fixed-Point Conversion:**
    * Converted the floating-point MATLAB models to an efficient **Fixed-point representation** to optimize hardware performance.
3.  **Hardware Implementation:**
    * Used **Simulink HDL Coder** to automatically generate synthesizable Verilog HDL code.
    * **Tool:** Xilinx Vivado.
4.  **Verification (Closed-Loop):**
    * **Data Source:** Pre-recorded ECG samples from the **PhysioNet** database.
    * **Simulation:** Performed functional verification via **Testbench** in Vivado.
    * **Hardware Test:** Validated on-chip processing using **Integrated Logic Analyzer (ILA)**.
    * **Analysis:** Both Testbench simulation results and ILA captured data were **exported to CSV format** and imported back into **MATLAB** for comparison against the golden model.

## 📊 Results
* **Raw Signal:** Noisy ECG data with visible baseline drift and 50Hz superposition.
* **Filtered Signal:**
    * Baseline wander stabilized by the Elliptic HPF.
    * High-frequency artifacts smoothed by the Elliptic LPF.
    * 50Hz interference suppressed by the Notch filter.
* **Performance Metrics:** Achieved high-fidelity output with a low **Mean Squared Error (MSE)** and improved **Signal-to-Noise Ratio (SNR)**.

## 📂 Repository Contents
* `MATLAB_Design/`: MATLAB scripts (.m) for CSV processing and Simulink models (.slx).
* `RTL/`: Generated Verilog source code.
* `Simulation/`: Testbench files and CSV outputs.
* `Docs/`: Project report and ILA waveform screenshots.


