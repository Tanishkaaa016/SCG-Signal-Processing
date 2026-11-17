# SCG Signal Processing: Heart Rate and Respiration Rate Estimation

## Overview

This project focuses on the design and implementation of digital filters for estimating heart rate (HR) and respiration rate (RR) from Seismocardiography (SCG) signals.

SCG is a non-invasive technique that captures small mechanical vibrations of the chest caused by cardiac activity and respiration. The project uses an MPU6050 accelerometer connected to an ESP32 to acquire these vibrations, followed by digital signal processing to separate the cardiac and respiratory components.

**Current status:** The Python signal-processing pipeline has been developed and tested using simulated SCG data. ESP32–MPU6050 hardware integration and validation with real recordings are pending.

## Objectives

- Explore and analyse SCG signals in the time and frequency domains.
- Design and compare different digital filters.
- Separate cardiac and respiratory components.
- Estimate heart rate in beats per minute (BPM).
- Estimate respiration rate in breaths per minute.
- Explore causal filtering for real-time processing.
- Prepare the pipeline for future ESP32-based data acquisition.

## Hardware (planned)

- ESP32 DevKit V1
- MPU6050 (GY-521) accelerometer module

## Technologies Used

- Python 3.9
- NumPy and Pandas
- SciPy
- Matplotlib and Seaborn
- Scikit-learn
- PyWavelets
- Jupyter Notebook

## Project Structure

```text
SCG-Signal-Processing/
│
├── data/
│   ├── raw/
│   │   └── sample_scg.csv
│   └── processed/
│
├── notebooks/
│   ├── 01_explore.ipynb
│   ├── 02_filters.ipynb
│   └── 03_rates.ipynb
│
├── src/
│   └── scg_pipeline.py
│
├── figures/
├── requirements.txt
├── .gitignore
└── README.md
```

## Methodology

### 1. Signal Acquisition and Exploration
- Load accelerometer data from CSV.
- Convert raw accelerometer readings into acceleration in g.
- Estimate sampling frequency from timestamps.
- Examine the signal using time-domain plots, FFT, Power Spectral Density (PSD), and spectrograms.

### 2. Digital Filter Design
- Design and compare Butterworth, Chebyshev Type I, Chebyshev Type II, and Elliptic IIR filters.
- Explore FIR filtering using the Kaiser window and Parks–McClellan method.
- Apply filtering for cardiac and respiratory frequency components.
- Compare causal and zero-phase filtering.
- Explore decimation from 250 Hz to 25 Hz for respiration processing.

### 3. Heart Rate Estimation
- Apply a 0.8–20 Hz Butterworth bandpass filter.
- Extract the signal envelope using the Hilbert transform.
- Detect heartbeat peaks.
- Estimate heart rate from the intervals between detected peaks.

### 4. Respiration Rate Estimation
- Downsample the signal to 25 Hz.
- Apply a 0.1–0.5 Hz Butterworth bandpass filter.
- Detect respiratory peaks.
- Estimate respiration rate from breathing-cycle intervals.

### 5. Signal Quality Assessment
The pipeline evaluates recording duration, sampling frequency, sampling jitter, signal statistics, and the presence of finite values. These checks are preliminary engineering checks, not a clinically validated signal-quality assessment.

### 6. Real-Time Processing Exploration
Causal filtering and block-based processing have been explored using simulated data. Further validation is required before considering the implementation ready for live hardware use.

## Preliminary Results

The pipeline was tested using a 30-second simulated recording sampled at 250 Hz.

| Parameter | Preliminary result |
|---|---:|
| Sampling frequency | 250 Hz |
| Recording duration | Approximately 30 seconds |
| Estimated heart rate | 73.71 BPM |
| Estimated respiration rate | 15 breaths/min |

These are results from simulated data, not measurements from a person or validated hardware recordings. They should not be interpreted as clinical performance results.

## Installation

Clone the repository:

```bash
git clone https://github.com/Tanishkaaa016/SCG-Signal-Processing.git
cd SCG-Signal-Processing
```

Create and activate a virtual environment:

```bash
python -m venv .venv
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

## Running the Project

Open the project folder in VS Code and run the notebooks in order:

1. `01_explore.ipynb` – Signal exploration and preprocessing.
2. `02_filters.ipynb` – Digital filter design and comparison.
3. `03_rates.ipynb` – Heart rate and respiration rate estimation.

The main processing functions are available in `src/scg_pipeline.py`.

## Future Work

- Complete ESP32 and MPU6050 hardware integration.
- Record and analyse real SCG signals.
- Validate sampling frequency and signal quality using hardware recordings.
- Evaluate HR and RR estimates against suitable reference measurements.
- Improve real-time processing and peak detection.
- Test the pipeline across multiple recordings.

## Disclaimer

This project is an academic signal-processing implementation and is not intended for medical diagnosis or clinical decision-making.
