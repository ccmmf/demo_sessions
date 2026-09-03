# Running Sipnet ensembles for a MAGiC inventory

In this session, we will walk through the setup and execution of ensemble Sipnet simulations
using data from the monitoring pipeline.


## File locations 

A few variables specific to my own environment to help me navigate the demo.
These will take different values on your system.

```sh
export DEMO_DIR=/project/60007/cblack/ensemble_demo_20260903
export CONDA_DIR=/project/60007/cblack/.conda/envs
```

## Set up environment (once)

(do ahead; it takes some time)

As documented in https://github.com/ccmmf/magic-training/blob/main/CARB-PEcAn-setup.md:

```sh
module load awscli_v2
export AWS_PROFILE=magic
aws s3 cp s3://carb/deploy/setup-pecan-env.sh ./
bash setup-pecan-env.sh 1.17 /project/60007/cblack/.conda/envs/pecan-all-1.17
```


## Activate existing environment v1.17

```sh
# module load awscli_v2
export AWS_PROFILE=magic
conda activate ${CONDA_DIR}/pecan-all-1.17
```


## Clone workflow repo (once)

```sh
mkdir -p "$DEMO_DIR" && cd "$DEMO_DIR"
git clone https://github.com/ccmmf/workflows.git
cd workflows
# git checkout main
git fetch  && git checkout carb-demo-20260903 
```


## Fetch demo data

```sh
aws s3 sync s3://carb/management/crops/ data_raw/management/crops/
aws s3 sync s3://carb/management/harvest/ data_raw/management/harvest/
aws s3 sync s3://carb/management/planting/ data_raw/management/planting/
aws s3 sync s3://carb/management/phenology/ data_raw/management/phenology/
aws s3 sync s3://carb/management/tillage/ data_raw/management/tillage/
aws s3 sync s3://carb/management/irrigation/ data_raw/management/irrigation/
aws s3 sync s3://carb/data_raw/ERA5_CA_nc/ data_raw/ERA5_CA_nc/
aws s3 sync s3://carb/IC/ data_raw/IC/
aws s3 sync s3://carb/data/workflows/phase_3/demo_data_20260903/ demo_data_20260903/
```


## Explore CLI layout

Workflow execution is controlled by  the `magic-ensemble` command-line interface
and proceeds in several stages: 

* declaring input and output locations,
* configuring run settings,
* Preprocessing inputs,
* running the model.

The CLI help shows the available commands:

```sh
./magic-ensemble -h
```

The commands available in the CLI are defined by the `steps` section of 
the workflow manifest, which we can inspect:

```sh
vim workflow/workflow_manifest.yaml
```

Key points:
* Each command consists of a set of steps, each of which calls one script.
* All tuning of script behavior happens via command-line arguments that are
	managed by magic-cli. 
* The step declaration lists all the inputs (files), nonfile arguments (params),
	and output files that the script accepts.
* inputs, parameters, and outputs are listed as key-value pairs. The key is always
	the exact command-line argument name the script expects,
	i.e. a step called as `./foo.R --indir path-to-bar` would be declared as
	`{script: foo.R, inputs: {indir: path_to_bar}`
* The value passed to an input or output must itself be a key in the manifest's 
	`paths` block. This is verbose to read, but means you can override any value
	you need to by setting its path in your user config file (more on that in a minute).
* The manifest additionally declares some values that are fixed across the whole workflow,
	e.g. s3 bucket names. These too can be overridden in the user config if you
	need it.
* When called with `--verbose`, `magic-ensemble` reports the exact call issued for
	each script, including all arguments.
* All console output is logged automatically.


## Explore configuration file

The user config contains a few values you will need to set every time (e.g. run_dir),
plus many more that you are likely to set once and forget them.

```sh
vim config.yaml
```

- outdir
- sipnet version
- locations of shared datasets
- queue submission info
- run size / replication levels

## Prepare inputs

```sh
cp demo_data_20260903/demo_config.yaml config.yaml
cp -R data_raw/IC/soil_moisture/ demo-run-20260903/IC_prep/soil_moisture
./magic-ensemble --verbose --config config.yaml prepare-example-3
```
- preprocesses input data
	- weather from netcdf to Sipnet clim file
	- management data from Parquet to Sipnet event file
	- initial conditions for each site
- inserts configured values into settings.xml

- key file types:
	- met inputs (ERA5): download once as netcdf, convert to clim, store
	- Initial conditions: download once as statewide files, build site ncs as needed
		- download is more complicated than necessary; converting these to parquet
	- site list: provided as CSV (+ scripts to help regenerate)
	- model parameter distributions (pfts): provided; calibration still in progress

## Set up PEcAn run directory and exucute Sipnet

```sh
./magic-ensemble --verbose --config config.yaml run-ensembles
```

- Builds a statistical sampling design and records it for future reference
- Assigns samples from input-based (ensembled files) and parameter-based 
	(draws from posterior distributions of the model parameters) uncertainty
- Constructs the PEcAn run directory (separate dir for each site-ensemble member
	combo)
- For runs where crops change during the simulation period, adds sub-directories
	to run each crop segment separately, resetting model parameters to the appropriate
	crop when changing segments.
- Kicks off Sipnet runs inside each site-ensemble dir
- Collects model outputs as netcdf, collects log files and run metadata in one place

## Analyze the result!

The output from a `magic-ensemble` run is ready to send on to `magic-downscaling`,
but since this one is a small six-site test we can visualize it directly.

```sh
./tools/plot_agb.R demo-run-20260903/output demo-run-20260903
```
