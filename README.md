# Training materials for the MAGiC system

This repository contains notebooks that demonstrate usage of the
Modeling and Analysis of Greenhouse Gases in Cropland (MAGiC) system,
a cropland carbon monitoring and scenario analysis tool for California that has
been built on multi-source remote sensing fused with
[CADWR land use maps](https://data.cnra.ca.gov/dataset/statewide-crop-mapping),
and simulated using the [Sipnet](https://pecanproject.github.io/sipnet/)
biogeochemistry model using the
[PEcAn](pecanproject.org) ecosystem modeling toolbox.

Each notebook presents a self-study demo to be completed asynchronously.
These will then be discussed further in live training sessions.

If you have questions about any of this material, you can:

* Reach out to the author listed in the notebook
* Open an issue here
* Email Chris Black <chris@poolsandfluxes.com>

Topic | URL | Who for | Objectives | Notes
--|--|--|--|--
Environment setup | https://github.com/ccmmf/magic-training/blob/main/CARB-PEcAn-setup.md | everyone | Configure AWS and Conda | You will need this complete for all later notebooks
PEcAn demo 1: single-site ensemble | https://github.com/ccmmf/magic-training/blob/main/pecan-demo-1.md | everyone | Run a simple model using PEcAn | Uses Sipnet 1.3
PEcAn demo 2: uncertainty analysis | https://pecanproject.github.io/pecan-documentation/develop/run-demo-2.html | everyone | Adjust PEcAn settings, understand outputs | Uses Sipnet 1.3
|||||
MAGiC grass & tree ensembles | https://github.com/ccmmf/demo_sessions/blob/main/20260609/2a_grass_demo.md | MAGiC users | Run a simple MAGiC workflow | 
MAGiC row crop demo | https://github.com/ccmmf/workflows/blob/main/examples/3_rowcrop/README.md | MAGiC users | Run Sipnet with RS monitoring data | placeholder notebook
MAGiC downscaling demo | https://github.com/ccmmf/downscaling/blob/develop/docs/intro_to_downscaling.qmd | MAGiC users | Create carbon maps from a model ensemble |
Aggregation | | | | |					
| | | | | |
Monitoring Part 1: LandIQ | | | | |
Monitoring Part 2: Phenology | | | | |
Monitoring Part 3: Tillage + NCC + N | | | | |
Monitoring Part 4: Irrigation | | | | |
Monitoring Part 5: Crop Scenarios | | | | |
Monitoring Part 6: All other Scenarios | | | | |
| | | | | |
Formal Training: Monitoring | | | | |
Formal Training: Inventories | | | | |
Formal Training: Scenario Configuration | | | | |
Formal Training: Projections | | | | |

