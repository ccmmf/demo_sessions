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
* Email Chris Black: <chris@poolsandfluxes.com>

Topic| Audience | Objectives | Notes
--|--|--|--
[Environment setup](https://github.com/ccmmf/magic-training/blob/main/CARB-PEcAn-setup.md) | Everyone | Configure AWS and Conda | You will need this complete for all later notebooks
| | | | |
[PEcAn demo 1: single-site ensemble](https://github.com/ccmmf/magic-training/blob/main/pecan-demo-1.md) | Everyone | Run a simple model using PEcAn | Uses Sipnet 1.3, which has some input files differences from the Sipnet v2 used for MAGiC runs.
[PEcAn demo 2: uncertainty analysis](https://pecanproject.github.io/pecan-documentation/develop/run-demo-2.html) | Everyone | Adjust PEcAn settings, understand outputs | Uses Sipnet 1.3
| | | | |
[MAGiC grass & tree ensembles](https://github.com/ccmmf/demo_sessions/blob/main/20260609/2a_grass_demo.md) | MAGiC users | Run a simple MAGiC workflow | 
[MAGiC row crop demo](https://github.com/ccmmf/workflows/blob/main/examples/3_rowcrop/README.md) | MAGiC users | Run Sipnet with RS monitoring data | placeholder notebook
[MAGiC downscaling demo](https://github.com/ccmmf/downscaling/blob/develop/docs/intro_to_downscaling.qmd) | MAGiC users | Create carbon maps from a model ensemble |
MAGiC Aggregation | | TK | |					
| | | | |
Monitoring Part 1: LandIQ | Data managers | | |
Monitoring Part 2: Phenology | Data managers | | |
Monitoring Part 3: Tillage + NCC + N | Data managers | | |
Monitoring Part 4: Irrigation | Data managers | | |
| | | | |
Monitoring Part 5: Crop Scenarios | Scenario developers | | |
Monitoring Part 6: All other Scenarios | Scenario developers | | |
| | | | |
Formal Training: Monitoring | | | |
Formal Training: Inventories | | | |
Formal Training: Scenario Configuration | | | |
Formal Training: Projections | | | |


### Audiences

These materials have been developed with four roles in mind:
* **Everyone:** If you will use PEcAn or MAGiC at CARB, you need to know this.
	This material assumes familiarity with code notebooks and access to CARB's
	cluster computing environment. No prior experience using PEcAn, Sipnet,
	or MAGiC is needed.
* **MAGiC users:** Staff or stakeholders who will run models, define management
	scenarios, analyze results, or otherwise need to understand how MAGiC
	operates.
	This material assumes you have already worked through the setup
	and basic demo notebooks, have access to preprocessed input datasets,
	and are comfortable working on the command line in CARB's cluster computing
	environment.
* **Data managers:** Staff who will update, reprocess, or implement changes to the
	monitoring datasets.
	Users who intend to use the data in its existing form without rerunning its
	processing pipelines do not need to study these notebooks.
	This material assumes you have already worked through the notebooks for
	MAGiC users and that you are comfortable with scripted batch manipulation of
	large spatial datasets.
* **Scenario developers:** Staff who will define or implement projection
	scenarios.
	This material will assume you have already worked through the notebooks for
	MAGiC users; assumptions beyond that are to be determined.
