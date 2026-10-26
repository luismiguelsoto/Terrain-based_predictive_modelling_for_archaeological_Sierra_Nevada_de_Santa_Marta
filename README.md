Terrain-based predictive modelling for archaeological prospection in the Sierra Nevada de Santa Marta, Colombia (under review)
--------------------------------------------------------------
This repository contains the code for a terrain-based predictive modelling framework designed to evaluate whether associations between archaeological terraces and terrain learned in one surveyed basin transfer to another watershed. The protocol integrates (1) terrain predictors grouped into local morphology, hillslope hydrology and basin position, (2) predictor selection under spatial cross-validation with nested selection, (3) logistic regression and an equal-weight ensemble of random forest, gradient boosting and XGBoost, (4) survey negatives compared with random background points for discrimination and calibration, and (5) external evaluation against slope alone with pre-specified criteria, univariate directions, permutation importance, accumulated local effects and the area of applicability. The protocol is applied to Tairona-period agricultural terraces in the Sierra Nevada de Santa Marta, Colombia, with training in the Río Frío basin and external evaluation at Buritaca 200.

Data Archive:
----------------------------------
- The eight spatial inputs are archived in Zenodo under a CC BY 4.0 licence (https://doi.org/10.5281/zenodo.23652371).
- The R notebook downloads them from that record at the start of each run and checks every file against the MD5 checksum that Zenodo lists for it.

Repository Structure:
----------------------------------
1. RData:
   - Contains copies of seven of the eight spatial inputs. Each file holds one R object, and raster objects are stored as packed terra objects. The notebook uses a local copy only when the download from Zenodo fails, and only when its MD5 matches the Zenodo record:
     • dem_full (Regional elevation model, SRTM elevations resampled to 12.5 m)
     • poly52 (Río Frío survey polygon, 52.2 km²)
     • grid_raw (Half-hectare survey grid, 10,733 cells, 1,131 with terraces)
     • ref_all_structures (3,500 recorded structures used as visibility observers, including the 2,526 terraces inside the Río Frío survey polygon)
     • buritaca_200 (159 terraces at Buritaca 200, used for external evaluation)
     • rivers (Hydrographic network, used only for maps)
     • canopy_raw (ETH canopy height, secondary land cover model)
     • s2_raw (Sentinel-2 indices, secondary land cover model, 153 MB, available only in the Zenodo record)

2. R Code Files:
   - The main R notebook (AP_PREDICTIVE_MODEL_V13.Rmd) contains the code to:
     a) Load required packages, configure the environment and check the CUDA device.
     b) Download the eight inputs from Zenodo, verify their MD5 checksums and derive the terrain predictors with WhiteboxTools.
     c) Perform predictor selection, model fitting, spatial and nested cross-validation, calibration and external evaluation.
     d) Generate Figures 1 through 11 and Tables 1 through 4 as presented in the manuscript, with the numerical summary of Figures 2 through 11.
     e) Record the computing environment, the analysis parameters, every pipeline step and the fingerprints that a rerun must reproduce.

3. Rendered Report:
   - AP_PREDICTIVE_MODEL_V13.pdf is the report of the reference run, with every figure, table and numerical summary.

Software and Key Package Versions:
----------------------------------
- R version: 4.6.1
- Python XGBoost version: 3.4.1 (CUDA build)
- WhiteboxTools version: 2.4.0, run with one process

Key R packages used in this project include (versions derived from sessionInfo):
    • CAST: version 1.1.2
    • digest: version 0.6.39
    • dplyr: version 1.2.1
    • FNN: version 1.1.4.1
    • gbm: version 2.3.1
    • ggnewscale: version 0.5.2
    • ggplot2: version 4.0.3
    • ggrepel: version 0.9.8
    • ggspatial: version 1.1.11
    • knitr: version 1.52
    • patchwork: version 1.3.2
    • pROC: version 1.19.1
    • PRROC: version 1.4
    • ragg: version 1.5.2
    • ranger: version 0.18.0
    • reticulate: version 1.47.0
    • rnaturalearth: version 1.2.0
    • rnaturalearthdata: version 1.0.0
    • scales: version 1.4.0
    • sf: version 1.1.3
    • spdep: version 1.4.2
    • systemfonts: version 1.3.2
    • terra: version 1.9.50
    • tibble: version 3.3.1
    • tidyr: version 1.3.2
    • tidyterra: version 1.3.0
    • viridisLite: version 0.4.3
    • whitebox: version 2.4.3
    • xgboost: version 3.2.1.1

Hardware Requirements:
----------------------------------
- XGBoost runs on an NVIDIA GPU through the Python CUDA build, with a seed for every fit, 16 threads for ranger and XGBoost, and no silent fallback to the CPU.
- Set the environment variable `XGB_PYTHON` to the Python executable that holds the CUDA build of XGBoost and numpy.
- The notebook stops if XGBoost cannot run on the CUDA device, so this version cannot run without an NVIDIA GPU.
- The reference run used an NVIDIA GeForce RTX 5080, 16 physical cores and 93.5 GB of RAM.

Getting Started:
----------------------------------
1. Clone or download this repository.
2. Open AP_PREDICTIVE_MODEL_V13.Rmd in RStudio, and set Tools > Global Options > General > Graphics > Backend to AGG.
3. Set `XGB_PYTHON` as described above. WhiteboxTools is installed automatically if it is missing, and the report records its version.
4. Knit the notebook. The default output is PDF, which needs a LaTeX distribution with xelatex such as TinyTeX. HTML and Word outputs are also available.
5. The notebook downloads the eight inputs from Zenodo, about 167 MB, when the run starts. Without internet, place the eight RData files in one folder and set it as the environment variable `AP_PROJECT_DIR`, for example with `Sys.setenv(AP_PROJECT_DIR = "path/to/RData")`.
6. Compare the reproducibility record printed under Fig. 2 with the reference values given below.

Reproducibility:
----------------------------------
- The specification of the analysis is frozen and recorded with a SHA-256 hash, shown in Fig. 2 and in the Configuration section of the report.
- XGBoost uses a seed for every fit on the CUDA device, ranger and GBM use fixed seeds, and WhiteboxTools runs with one process. The notebook computes the hydrology twice in each run and reports whether both results are identical.
- The reference run produced these values, and a rerun with the same software versions should reproduce them:
     • Specification hash (SHA-256): e40720e0d80a3ace378c98f5b5d854f0cfeb03c95584259586132f49e2b5aaaf
     • Results fingerprint (SHA-256): 7021ca54d07403f2ca3e2e49a5a88ac17279cea7d67af56e182bf65662a7d898
     • Hydrology fingerprint (SHA-256): 00404d7f57ad20a4b70570c57530825edaa16754f107fbbabc468f79a966629b

Manuscript Summary:
----------------------------------
Predictive models can guide archaeological survey only if the relationships they learn hold beyond the area that trained them. This study tests that condition for Tairona-period terraces in the Sierra Nevada de Santa Marta, Colombia. Models were trained on a full-coverage survey of 52.2 km² in the Río Frío basin, which recorded 2,526 terraces, and applied unchanged to 159 terraces at Buritaca 200, in a separate watershed. Terrain predictors from SRTM-derived elevations resampled to 12.5 m, describing local landforms, hillslope drainage and basin position, were selected under spatial cross-validation, fitted with logistic regression and a machine-learning ensemble, and compared with slope alone. Inside Río Frío, height above large channels gave the largest gain, raising the ensemble’s discrimination (AUC) from 0.675 to 0.731. At Buritaca 200 its relationship with terraces reversed, and the model including it fell below slope alone (AUC 0.819 against 0.873), whereas the local landform model matched slope (0.879). Training with surveyed cells lacking terraces instead of background points barely changed discrimination but yielded probabilities consistent with recorded frequencies. Internal accuracy did not anticipate transfer. In this test, local landform models checked against slope alone offered a sounder basis for prospection than richer models tuned to one drainage.

For questions or further information, please contact: lms313@pitt.edu
