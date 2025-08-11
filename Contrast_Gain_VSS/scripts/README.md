# MEG Data Processing Pipeline for Visual Snow Study

## Overview

This repository contains a complete pipeline for processing and analyzing MEG data from a contrast gain control in the visual snow syndrome study. The pipeline includes:

1. ICA preprocessing and artifact removal
2. Epoch creation and cleaning
3. Power spectral density (PSD) analysis
4. Contrast response function modeling

## Scripts

### 1. Preprocessing and ICA

- `1_preprocessing_and_ica.py`: Performs ICA decomposition and identifies artifact components (ECG, EOG)
- `2_apply_ica.py`: Applies ICA cleaning to remove identified artifacts from the raw data

### 2. Epoching

- `3_epoching_and_epochs_cleaning.py`: Segments continuous data into 2-second epochs and provides tools for manual epoch rejection

### 3. Spectral Analysis

- `4_psd_analysis.py`: Computes power spectral density for different contrast conditions

### 4. Modeling

- `5_model_fitting.py`: Fits Naka-Rushton contrast response functions to the PSD data

## Outputs

Processed data is saved in the following directory structure:
- ICA results: `{path_to_subjects_folders}/{subjects_folder}/{ICA}`
- Epochs and analysis: `{path_to_subjects_folders}/{subjects_folder}/{epochs}`

## Requirements

- Python 3.x
- MNE-Python
- NumPy, Pandas, Matplotlib
- lmfit (for model fitting)
- Additional MNE dependencies (scipy, sklearn)

## Usage

1. Run preprocessing scripts in order:
   - `1_preprocessing_and_ica.py` → `2_apply_ica.py` → `3_epoching_and_epochs_cleaning.py`
   
2. Perform spectral analysis:
   - `4_psd_analysis.py`
   
3. Fit contrast response models:
   - `5_model_fitting.py`

## Notes

- Subject IDs are specified at the beginning of each script
- Paths may need to be modified for different systems
- Scripts include comprehensive logging and report generation
- Visual inspection steps are included for quality control
- The pipeline assumes MaxFiltered data as input (tSSS and movement compensation applied)

## Configuration

Key parameters can be adjusted in the **CONFIGURATION PARAMETERS** section of each script, including:
- Filter settings
- ICA parameters
- Epoch timing
- Frequency bands of interest
- Model fitting constraints