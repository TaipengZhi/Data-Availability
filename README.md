# Reproducibility package — Target-guided resonance-band selection for bearing fault-harmonic measurement under strong noise

This archive supports the reported simulations and analyses. The CWRU and XJTU-SY data are third-party public datasets and are **not** redistributed here; download links are listed below.

## Directory layout

- `matlab/` — all MATLAB scripts used to generate the simulated data and run every method (B1–B8, the core THNR adjudication B3, the full VMD procedure, calibration, validation, pressure tests and input-parameter sensitivity).
- `analysis/gen_v4.py` — builds the manuscript `paper_MST_v4.docx`; **no experimental numbers are hard-coded**. Every table reads the derived result files / CSV listed in `stats/`.
- `analysis/make_b8_figs.py` — rebuilds the Pd heat-map and ef/NRMSE curves including B8 (IESFOgram).
- `stats/` — derived tables (white-noise summary, ROC/AUROC per speed-SNR-method, paired bootstrap, rescue/convergence audit).
- `figures/` — vector (PDF) versions of the seven result figures.

## Frozen configuration (provenance)

- Configuration hash: `1EE51124C88742FB18D9056B291B514422309C506DDA4203A3398B4E0EEF73EC`
- Sampling rate `fs = 12 000 Hz`; resonance carrier `f_res = 3600 Hz` (boundary stress case 3000 Hz).
- Minimum bandwidth `B_min = 1000 Hz`; candidate centre range 1000–4800 Hz; retained multiresolution bands = 5 (all feasible).
- Lambda_K = 0.5; K list = [2,3,4,5]; alpha grid [0.05..16] × 12; VMD fixed budget 500 iterations.
- Speeds = [600, 1200, 1800] r/min; SNRs = [-15,-10,-5,0,+5] dB.
- BPFO used: 35.85 / 71.70 / 107.54 Hz.
- nRep = 50 fault realisations; nCal = 100, nVal = 100 normal records (pressure tests: 50/50/50).
- Method column order in `white_*.mat` (N×7): 0=Full(Proposed), 1=B2, 2=B3, 3=B4, 4=B5, 5=B1, 6=B6.
- All random seeds are fixed; run scripts in the listed order to regenerate.

## Signal-model note

`fz_sim_signal.m` uses `fix_period = true` by default, producing impacts at `0, Tf, 2Tf, ...`.
`fz_period_sensitivity.m` compares this against the earlier off-by-one sequence.

## Third-party public data (not included)

- CWRU Bearing Data Center: https://engineering.case.edu/bearingdatacenter
- XJTU-SY accelerated life test (LDK UER204; Z=8, d=7.92 mm, D=34.55 mm): Lei Y, Han T, Wang B, Li N, Yan T, Yang J 2019, J. Mech. Eng. 55(16) 1–6, doi:10.3901/JME.2019.16.001

## How to reproduce

1. In MATLAB, from `matlab/`, run the runners in order:
   `fz_run_white` → `fz_run_b7_iesf` → `fz_run_b8_iesfog` → `fz_run_pressure` → `fz_run_uncertainty`.
2. Run `fz_gate_white` then `fz_rescue` for the audit numbers.
3. In Python, run `analysis/gen_v4.py` to rebuild the manuscript tables/figures.
