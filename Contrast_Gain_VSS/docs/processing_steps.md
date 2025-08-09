
MEG Processing Pipeline

1. Preprocessing (`1_preprocessing_and_ica.py`)
1. MaxFilter:
   - tSSS (temporal Signal Space Separation)
   - Head movement compensation
   - Bad channel interpolation

2. ICA Artifact Removal:
   - Bandpass filtering (1-140Hz)
   - Notch filtering (50Hz + harmonics)
   - Automatic detection:
     - ECG artifacts (max 3 components)
     - EOG artifacts (max 1 component each)
   - Manual inspection of components
   - Output: Cleaned continuous data

3. Apply ICA (`2_apply_ica.py`):
   - Manual component rejection
   - Saves cleaned continuous data

2. Epoching (`3_epoching_and_epochs_cleaning.py`)
- Epoch Creation:
  - 2-second epochs (50% overlap)
  - Start aligned to every 7th reversal cycle (~150ms intervals)
  - Baseline: -1.0 to 0s
  - Post-stim: 0 to 1.2s

- Artifact Rejection:
  - Automatic:
    - Gradiometer threshold: 4000e-13 T/m
    - High gamma (70-146Hz) power exclusion (2SD)
  - Manual visual inspection
  - Target: ~65 clean epochs per condition

3. Spectral Analysis (`4_psd_analysis.py`)
- Power Spectral Density:
  - Welch method (3s windows: 2s data + 0.5s zero-padding)
  - Frequency resolution: 0.33Hz
  - Target frequencies:
    - Fundamental: 6.666Hz (F1)
    - Harmonics: 13.333Hz (F2), 20.0Hz (F3), 26.666Hz (F4), 40.0Hz (F6), 53.333Hz (F8)

- Appelbaum Metric:
  - Signal power: Sum at stimulation harmonics
  - Baseline power: Average of surrounding frequencies (±0.333-0.666Hz)

4. Contrast Response Modeling (`5_model_fitting.py`)
- Naka-Rushton Function:
  
  R = Rmax*C²/(v²ˢ + C²ˢ) + b
  
  Where:
  - R = Response power
  - C = Stimulus contrast
  - Rmax = Maximum response
  - v = Semisaturation constant
  - s = Saturation exponent (key parameter)
  - b = Baseline

- Fitting:
  - Nonlinear minimization (Lmfit)
  - Parameter constraints:
    - v: 0-50
    - s: 0-5
    - b: ≥0

5. Statistical Analysis
- Group Comparisons:
  - Primary target: Saturation parameter (s)
  - Tests: Student's t-test or Mann-Whitney
  - Effect size: Cohen's d or r
- Correlations: Spearman's rank with visual discomfort scores


Quality Control
- ICA component reports (HTML)
- Topographic maps for each harmonic
- Model fitting plots with R² values
- Epoch rejection statistics