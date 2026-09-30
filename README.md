# PCA from Scratch: CO₂ Emissions in Africa

**Formative Assignment 1: Advanced Linear Algebra (Principal Component Analysis)**
**Group / Peer Pair:** `PCA PAIR TEAM 6`

## Overview
This project implements **Principal Component Analysis (PCA) from scratch using only NumPy and Matplotlib**. We apply it to CO₂ emissions and socio-economic data for 54 African countries from 2000 to 2020. The goal is to reduce 12 correlated features to a few principal components that keep most of the variance, and to interpret what those components reveal about **economic activity, population pressure and emissions** across the continent.

## Repository Contents
| File | Description |
|---|---|
| `PCA_Formative_1_CO2_Emissions_Africa.ipynb` | Completed notebook with all code, outputs, plots and explanations |
| `co2 Emission Africa.csv` | Dataset used in the notebook |
| `<Task_Sheet>.pdf` | Group contribution / task sheet |
| `README.md` | This file |

## Dataset
- **Source:** CO₂ Emissions in Africa (country-level, 2000–2020)
- **Size:** 1,134 rows × 20 columns (54 countries × 21 years)
- **Features:** population, GDP per capita (USD and PPP), land area, and CO₂ emissions (Mt) by sector (transportation, electricity/heat, manufacturing/construction, buildings, industrial processes, land-use change and forestry, bunker fuels, other fuel combustion)
- **Missing values:** `NA` in 10 of the 20 columns
- **Non-numeric columns:** `Country`, `Sub-Region`, `Code`

## Method
1. **Load and inspect:** read the CSV with `np.genfromtxt` and detect the missing and non-numeric columns.
2. **Clean the data:**
   - Drop *Fugitive Emissions* (70.9% missing).
   - Exclude `Year` and the three total columns (*Energy*, *Total CO₂ incl./excl. LUCF*), since they are sums of other columns.
   - Drop South Sudan's rows from before 2012, because the country did not exist yet.
3. **Impute:** fill gaps with the country's own median. When a country has no values at all for a feature, use its sub-region's median for the same year.
4. **Encode:** one-hot encode `Sub-Region` and use it to colour the plots and validate the clusters.
5. **Transform:** apply a signed log, `sign(x)·log(1+|x|)`, to reduce heavy skew. It also handles the negative land-use values.
6. **Standardize:** `Z = (X − u) / o`.
7. **Covariance matrix:** `Z^T Z / (n − 1)`.
8. **Eigendecomposition:** `np.linalg.eigh`, then sort the eigenvalues and eigenvectors in descending order.
9. **Choose components dynamically:** take the smallest *k* whose cumulative explained variance reaches **90%**.
10. **Project and visualize:** compute `Z · W_k`, then plot before vs after PCA.
11. **Optimize (Task 3):** vectorized, float32 and chunked implementations, benchmarked up to 1,000,000 rows.

## Key Results
| Metric | Value |
|---|---|
| Final data | 1,122 rows × 12 features |
| PC1 | 57.8% of variance: **economic activity / emissions footprint** |
| PC2 | 22.2% of variance: **population pressure vs wealth per capita** |
| Components selected | **4 of 12 (91.5% variance retained)** |
| Dimensionality reduction | 12 → 4 features (−67%) |
| Speed-up vs naive PCA | ~180–300× (1M rows in under 0.5 s) |

Without being given region labels, PCA separates **Northern and Southern Africa** (the high industrial emitters) from Eastern, Middle and Western Africa. Along PC2, it separates large, populous, lower-income countries with land-use emissions (e.g. DR Congo, Ethiopia) from small, wealthier island states (e.g. Seychelles, Mauritius).

## How to Run
1. Clone the repository:
   ```bash
   git clone https://github.com/Abigail-Salem-Tendo/Team_6_PCA.git
   ```
2. Open `PCA_Formative_1_CO2_Emissions_Africa.ipynb` in Jupyter or Google Colab.
3. Make sure `co2 Emission Africa.csv` is in the same folder. In Colab, upload it to the session.
4. Run all cells from top to bottom.

**Requirements:** Python 3, `numpy`, `matplotlib` (no other libraries).
