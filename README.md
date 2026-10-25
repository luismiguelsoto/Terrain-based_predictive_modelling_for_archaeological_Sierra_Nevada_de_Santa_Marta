Terrain-based predictive modelling for archaeological prospection in the Sierra Nevada de Santa Marta, Colombia (under review)
--------------------------------------------------------------
This repository contains code and data for a terrain-based predictive modelling framework designed to evaluate whether associations between archaeological terraces and terrain learned in one surveyed basin transfer to another watershed. The protocol integrates (1) terrain predictors grouped into local morphology, hillslope hydrology and basin position, (2) predictor selection under spatial cross-validation with nested selection, (3) logistic regression and an equal-weight ensemble of random forest, gradient boosting and XGBoost, (4) survey negatives compared with random background points for discrimination and calibration, and (5) external evaluation against slope alone with pre-specified criteria, univariate directions, permutation importance, accumulated local effects and the area of applicability. The protocol is applied to Tairona-period agricultural terraces in the Sierra Nevada de Santa Marta, Colombia: training in the Río Frío basin and external evaluation at Buritaca 200.

Repository Structure:
----------------------------------
1. RData:
   - Contains the spatial data files used by the R notebook. Each file holds one R object, and raster objects are stored as packed terra objects:
     • dem_full (Regional 12.5 m elevation model)
     • poly52 (Río Frío survey polygon, 52.2 km²)
     • grid_raw (Half-hectare survey grid, 10,733 cells, 1,131 with terraces)
     • ref_all_structures (2,526 terraces recorded by the Río Frío survey)
     • buritaca_200 (159 terraces at Buritaca 200, used for external evaluation)
     • rivers (Hydrographic network, used only for maps)
     • canopy_raw (Canopy height, secondary land cover model)
     • s2_raw (Sentinel-2 indices, secondary land cover model, available in the Zenodo record)

2. R Code Files:
   - The main R notebook (AP_PREDICTIVE_MODEL) contains the code to:
     a) Load required packages, configure the environment and check the CUDA device.
     b) Load the RData inputs and derive the terrain predictors with WhiteboxTools.
     c) Perform predictor selection, model fitting, spatial and nested cross-validation, calibration and external evaluation.
     d) Generate Figures 1 through 11 and Tables 1 through 4 as presented in the manuscript.

3. HTML Report:
   - AP_PREDICTIVE_MODEL.html contains the rendered output of the notebook, with the numerical summary of each figure.

Software and Key Package Versions:
----------------------------------
- R version: 4.6.1
- Python XGBoost version: 3.4.1 (CUDA build)

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
- Set the environment variable `XGB_PYTHON` to the Python executable that holds the CUDA build of XGBoost.
- Without a GPU, set `allow_xgb_cpu_fallback <- TRUE` in the parameters chunk. Values may then differ slightly from the reference fingerprint.

Getting Started:
----------------------------------
1. Clone or download this repository, and download s2_raw.RData from the Zenodo record into the RData folder.
2. Open the main R Markdown file in RStudio, and set Tools > Global Options > General > Graphics > Backend to AGG.
3. Set the folder with the RData files as the environment variable `AP_PROJECT_DIR`, for example with `Sys.setenv(AP_PROJECT_DIR = "path/to/RData")`.
4. Set `XGB_PYTHON` as described above. WhiteboxTools is installed automatically if it is missing.
5. Knit the notebook to HTML, or run its chunks sequentially, to reproduce the analysis and generate all figures and tables.
6. Check the reproducibility table printed with Figure 2. A faithful rerun reproduces the specification hash e40720e0d80a and the results fingerprint 753d0496dc0de77f36eb4e212c1b664f5fd4cd5a41aff3a54507a3e272b8a764.

Manuscript Summary:
----------------------------------
Predictive models can guide archaeological survey only if the relationships they learn hold beyond the area that trained them. This study tests that condition for Tairona-period terraces in the Sierra Nevada de Santa Marta, Colombia. Models were trained on a full-coverage survey of 52.2 km² in the Río Frío basin, which recorded 2,526 terraces, and applied unchanged to 159 terraces at Buritaca 200, in a separate watershed. Terrain predictors from a 12.5 m elevation model, describing local landforms, hillslope drainage and basin position, were selected under spatial cross-validation, fitted with logistic regression and a machine-learning ensemble, and compared with slope alone. Inside Río Frío, height above large channels gave the largest gain, raising the ensemble's discrimination (AUC) from 0.675 to 0.731. At Buritaca 200 its relationship with terraces reversed, and the model including it fell below slope alone (AUC 0.819 against 0.873), whereas the local landform model matched slope (0.879). Training with surveyed cells lacking terraces instead of background points barely changed discrimination but yielded probabilities consistent with recorded frequencies. Internal accuracy did not anticipate transfer. Local landform models, tested in a separate basin against slope alone, offer a sounder basis for prospection than richer models tuned to one drainage.

For questions or further information, please contact: lms313@pitt.edu
