# Virtual LC

A browser-based liquid chromatography simulator for method development, signal customization, and quantitative analysis — no installation required.

> **Author:** João Victor Basolli Borsatto · 2026

---

## Overview

Virtual LC is a single-file HTML application that simulates reversed-phase HPLC/UHPLC chromatography. It is designed for educational use, method scouting, and training purposes. All simulation runs entirely in the browser — there is no backend, no server, and no data is sent anywhere.

---

## Getting Started

1. Download `virtual_lc_v2.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. No installation, no dependencies, no internet connection required after loading

---

## Modules

The application has three independent modules accessible from the top navigation bar.

### 1. Method Optimization

Simulates chromatographic runs based on instrument and column parameters. Supports both **isocratic** and **gradient** elution modes.

**Mobile Phase parameters:**
- `%B Initial` — starting organic content
- `ΔB (Range)` — gradient range; set to 0 for isocratic
- `Analysis Time (tG)` — gradient duration (visible only in gradient mode)
- Organic solvent selection: Methanol, Acetonitrile, Ethanol, Isopropanol
- Flow rate, column temperature, and formic acid modifier (0.1%)

**Column parameters:**
- Stationary phase: C18, C8, C4
- Inner diameter: 4.6 / 2.1 / 1.0 / <0.5 mm
- Particle type: Fully Porous (FPP) or Superficially Porous (SPP)
- Particle size: 1.7 / 3 / 5 / 10 µm
- System volume and dead time

**Detector Setup:**
- Data points per minute (6–48 pts/min)
- Noise level (%) and baseline drift (%)

**Compounds:**
- Add up to 50 compounds with adjustable polarity
- Polarity determines retention order (LSS model)

**Outputs:**
- Preview each run with overlays: %B profile, pressure, temperature, flow
- Annotations: peak name, tR, height, FWHM, resolution (Rs)
- Export PNG, CSV, or ZIP batch
- **Compare mode**: overlay up to 4 runs simultaneously with individual colors and legend
- Send any run to Customize Chromatogram or Quantitative Analysis

---

### 2. Customize Chromatogram

Build a chromatogram manually by defining each peak individually.

**Parameters per peak:**
- Retention time (RT)
- Peak width (FWHM)
- Height
- Shape: symmetric, tailing (As > 1), or fronting (As < 1)
- Asymmetry factor (As)
- Peak color

**Signal Quality:**
- Pts/min
- Noise on/off + noise level (%)
- Baseline drift (%)

**Actions:**
- Generate and preview the chromatogram
- Annotate: TR, HEIGHT, FWHM, W BASE (4σ), AREA
- Export as PNG or CSV
- **Send to Quantitative Analysis** — transfers peaks and signal settings
- **Send Detector & Baseline to Method Development** — transfers pts/min, noise, and drift to the MO detector setup

---

### 3. Quantitative Analysis

Generates calibration series chromatograms for a defined concentration range.

**Calibration Setup:**
- Concentration-to-intensity relationship: Linear, Quadratic, or Logarithmic
- Maximum height for scaling peaks

**Peaks:**
- Same peak editor as Customize
- Each peak can be toggled as a **calibration peak** (scales with concentration) or fixed (e.g. internal standard)

**Calibration Points:**
- Add concentration points with custom units (mg/L, µg/mL, etc.)

**Replicates:**
- Optionally generate 2–5 replicates per calibration point
- Replicates include random variation in RT (±10%), width (±10%), and area (±15%)

**Signal Quality:**
- Noise and baseline drift settings (same as Customize)

**Outputs:**
- Preview each concentration level
- Export PNG, CSV, or both
- ZIP download with OniChrom-compatible naming convention: `QA_01_10mgL_PPM24.png`

---

## Compare Runs

In the **Method Optimization** module, click **COMPARE RUNS** in the top bar to enter compare mode.

- A selection bar appears below the chromatogram
- Click any **R1, R2, R3...** button to add that run to the comparison (up to 4)
- Each run is drawn with a distinct color (blue, red, green, amber)
- A legend identifies each run by label
- Click **CLEAR** to reset the selection
- Click **COMPARE RUNS** again to exit compare mode

---

## Sending Data Between Modules

| From | To | What is sent |
|---|---|---|
| Method Optimization (any run) | Customize Chromatogram | Peak RT, width, height, shape, color |
| Method Optimization (any run) | Quantitative Analysis | Peak RT, width, height, shape, color |
| Customize Chromatogram | Quantitative Analysis | Peaks + noise + baseline drift |
| Customize Chromatogram | Method Development | Pts/min + noise + baseline drift |

Use the **Send →** button next to each run in the run list to expand inline export options.

---

## Internal Simulation Factors

Click **⚙ FACTORS** in the top bar to access advanced simulation parameters:

| Parameter | Description |
|---|---|
| Base plates (N₀) | Theoretical plates at 3 µm FPP baseline |
| SPP plate gain | Efficiency multiplier for superficially porous particles |
| PSA prefactor/exponent (isocratic) | Polarity–retention power law (isocratic mode) |
| PSA prefactor/exponent (gradient) | Polarity–retention power law (gradient mode) |
| LSS S coefficient | Log-linear solvent strength factor |
| Temperature k reduction | Retention factor decrease per °C |
| Extra-column tailing factor | System volume contribution to tailing |
| Formic acid tailing/width reduction | Effect of 0.1% formic acid modifier |
| SPP fronting amplification | Fronting factor for SPP particles |
| Pressure base | Backpressure scaling factor |

These values can be reset to defaults at any time.

---

## Export File Naming

Files follow OniChrom-compatible naming:

- **Method Optimization:** `LC_Bi5_dB90_tG10_F04_T40_C18_21mm_FPP_3um_PPM24.png`
- **Quantitative Analysis:** `QA_01_10mgL_PPM24.png` / `QA_01_10mgL_PPM24_R1.png` (with replicates)

---

## Technical Notes

- Built entirely with vanilla HTML, CSS, and JavaScript
- Rendering uses the HTML5 Canvas API
- ZIP export uses [JSZip 3.10.1](https://stuk.github.io/jszip/) via CDN
- Fonts loaded from Google Fonts (IBM Plex Mono + IBM Plex Sans)
- No frameworks, no build step, no server

---

## Simulation Model

- **Retention** is modeled using the Linear Solvent Strength (LSS) theory
- **Peak shape** uses a modified Gaussian with asymmetry factor (As) via EMG approximation
- **Gradient elution** uses the LSS gradient retention equation with b = S·ΔΦ/tG
- **Plate count (N)** is estimated from column geometry, particle type, and size
- **Backpressure** is estimated from flow rate, column dimensions, and particle size
- **Extra-column effects** (system volume, tailing) are applied as post-peak modifiers

---

