# NMR Analysis Suite

A desktop application for processing and analyzing Bruker NMR data. Built with Python, PyQt6, NMRglue, and Matplotlib, it provides two independent analysis modules, T1 relaxation and CSP, accessible from a central launcher.

---

## Purpose

This tool was developed to streamline two common NMR data analysis workflows:

### T1 Relaxation
Processes pseudo-2D inversion-recovery experiments acquired on Bruker spectrometers. The user loads a Bruker dataset folder, selects an integration window over a peak of interest, and the application fits the inversion-recovery curve using the standard model:

```
I(t) = A × (1 − 2 × exp(−t / T1)) + C
```

The fit handles phase-inverted data automatically, provides R² and data-quality warnings, and exports results (fit parameters, raw trajectory, and fitted curve) to CSV.

### Chemical Shift Perturbation (CSP)
Loads a series of 1D Bruker spectra acquired at varying ligand concentrations and generates a stacked waterfall plot. The user selects a peak of interest, and the application fits a Gaussian to track the peak centre across all spectra. It then computes Δδ (chemical shift perturbation) and determines the dissociation constant **Kd** via the linear double-reciprocal method (Fielding 2007). Results and figures are exportable to CSV and PNG/PDF/SVG.

**Key features:**
- Interactive waterfall plot with rubber-band zoom, peak picking, and per-spectrum phase correction
- Automatic Gaussian peak fitting with fallback to spectral maximum
- Kd determination with R² reporting
- Black-and-white waterfall export suitable for publication

---

## Requirements

- Python 3.9 or newer
- Bruker processed data (TopSpin `pdata/1/` folder structure with `1r`/`2rr` files)

---

## Installation

**1. Clone the repository**
```bash
git clone https://github.com/your-username/NMR-Processing-Tool.git
cd NMR-Processing-Tool
```

**2. Create and activate a virtual environment (recommended)**
```bash
python -m venv venv

# macOS / Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

The `requirements.txt` installs:

| Package | Purpose |
|---|---|
| `nmrglue` | Reading Bruker binary data files |
| `numpy` | Numerical array operations |
| `scipy` | Curve fitting (Gaussian, inversion-recovery, linear regression) |
| `matplotlib` | Spectral plots and waterfall figures |
| `PyQt6` | Desktop GUI framework |

---

## Usage

Launch the application from the project root:

```bash
python main.py
```

The launcher window appears with two module buttons:

- **T1 Relaxation** — opens the T1 inversion-recovery analysis window
- **Chemical Shift Perturbation** — opens the CSP waterfall and Kd analysis window

### T1 Relaxation workflow
1. Click **Load Experiment** and select the Bruker dataset root folder (the folder containing numbered subfolders). The tool auto-detects the T1 experiment.
2. Use the spectral viewer to drag an integration window over your peak of interest.
3. Click **Fit T1**. The fitted curve and parameters appear in the results panel.
4. Click **Export CSV** to save results.

### CSP workflow
1. Click **Add Spectrum** (or load a folder of spectra) and assign a concentration and label to each.
2. Arrange spectra in the table; set the reference spectrum (typically the apo/no-ligand condition).
3. Click on the waterfall plot to place a peak marker, or type a ppm value directly.
4. Click **Run CSP Analysis** to open the results window showing Δδ vs. concentration and the Kd determination plot.
5. Export results as CSV or figures as PNG/PDF/SVG.

---

## License

See [LICENSE](LICENSE) for details.