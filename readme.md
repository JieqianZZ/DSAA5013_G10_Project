# DSAA5013 Group 10 Project: SRSNet Reproduction and Extension

## 1. Project Overview
This repository contains the final project for Group 10 in the DSAA5013 course at the Hong Kong University of Science and Technology (Guangzhou). The objective of this project is to reproduce **SRSNet**, a top-tier time-series forecasting model, and rigorously benchmark it against state-of-the-art models (including iTransformer, TimeMixer, and Amplifier) across multiple standard datasets (ETTh1, ETTh2, ETTm1, ETTm2, Weather, Electricity, Solar, Traffic).

Beyond the standard reproduction, our team implemented a complete extension pipeline, which includes:
1. **Automated Metrics Parser**: Custom Python scripts to automatically extract MAE/MSE metrics from generated log files and aggregate them into structured tables.
2. **Ablation & Parameter Sensitivity Pipeline**: Comprehensive stress-testing of hyperparameters (`patch_len`, `seq_len`, `d_model`) and internal spatial-temporal reassembly modules via automated shell scripts.
3. **Publication-Ready Visualizations**: Automated generation of 1x3 subplot parameter sensitivity charts using Seaborn, specifically designed to prevent visual distortion and align with top-tier conference standards.

---

## 2. Directory Structure
The repository is organized as follows:
```text
DSAA5013_G10_Project/
│── DSAA5013_G10_code.ipynb         # The core Jupyter Notebook executing the entire pipeline
│── SRSNet/                         # The cloned official codebase
│   ├── dataset/                    # Directory for all forecasting datasets
│   ├── scripts/                    # Shell scripts for executing various models 
│   ├── result/                     # Auto-generated logs and checkpoints after training
│   └── ...
└── Reproduce_Result/               # Custom directory containing parsed CSV/Excel results and generated plots
```

## 3. Setup & Data Preparation
The code is designed to be executed smoothly on Google Colab (A100/T4 GPU environments). By running the first section of the notebook, the environment will automatically:

1. Mount Google Drive for persistent storage.
2. Clone the official SRSNet framework.

3. Install dependencies from requirements.txt and downgrade to numpy<2.0.0 to ensure framework compatibility.

## 4. Running Baseline Experiments
In Sections 1 to 3 of our Notebook, we executed baseline comparisons across different models and forecasting horizons (96, 192, 336, 720). You can trigger these experiments by running the corresponding cells:

SRSNet: !bash ./scripts/multivariate_forecast/Electricity_script/SRSNet.sh

iTransformer: !bash ./scripts/multivariate_forecast/ETTh1_script/iTransformer.sh

TimeMixer: !bash ./scripts/multivariate_forecast/ETTm1_script/TimeMixer.sh

Amplifier: !bash ./scripts/multivariate_forecast/Traffic_script/Amplifier.sh

## 5. Automated Metrics Extraction (Newly Developed)
Since standard benchmarking frameworks save results deep within nested directories, we developed a custom automated data harvesting script in our Notebook:

It recursively searches for test_report*.csv within the result/ directory.

Uses Regular Expressions (re) to precisely match Horizons (96/192/336/720) and their corresponding MSE/MAE values.

Automatically concatenates the scattered data into a clean Pandas DataFrame and exports it as *_Final_Summary.csv or a comprehensive *_All_Results_Summary.xlsx, eliminating the need for manual log parsing.

## 6. Ablation & Parameter Sensitivity Pipeline (Newly Developed)
To deeply analyze the model's mechanics, we designed a rigorous stress-testing pipeline based on the Control Variable Method:

Structural Ablation Study:
We stripped the adaptive fusion module by overriding configurations (e.g., passing '{"use_adaptive_fusion": false}') to investigate whether spatial-temporal reassembly is strictly necessary for highly periodic data.

Automated Parameter Sensitivity:
We authored batch shell scripts (e.g., SRSnet_seq_len.sh, SRSnet_patch_len.sh) to systematically iterate through variables and isolate the specific impact of each hyperparameter on the MSE.


