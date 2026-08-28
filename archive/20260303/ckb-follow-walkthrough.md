# CARB PEcAn office hours #1, 2026-03-03

## What I actually did (with side diagnostics deleted)

record venv location to save typing (probably only needed because I didn't use ~/.conda because my /home is too full)
```sh
CONDA_ENV_DIR=/project/def-sponsor00/cblack/.conda/envs/pecan-all
```

Make a working directory, fetch conda tarball, unpack
```sh
mkdir /project/def-sponsor00/cblack/pecan-demo
cd /project/def-sponsor00/cblack/pecan-demo
rclone  copy ccmmf:carb/environments/pecan-all.tar.gz ./
mkdir -p "$CONDA_ENV_DIR"
tar -xzf pecan-all.tar.gz -C "$CONDA_ENV_DIR"
source "$CONDA_ENV_DIR"/bin/activate
conda-unpack
```

Get PEcAn repo
```sh
git clone https://github.com/pecanproject/pecan
cd pecan/documentation/tutorials/Demo_1_Basic_Run/
```

Work around a version complaint from the here package, then run notebook
```sh
Rscript -e 'install.packages("here")'
Rscript -e 'rmarkdown::render("run_pecan.qmd")'
```

Hooray! This worked for me.
Verified by downloading run_pecan.html and viewing locally.

## Second rendering: With Quarto this time

(installation method adapted from https://quarto.org/docs/download/tarball.html?version=1.9.27&idPrefix=download-pre)

```sh
mkdir "$CONDA_ENV_DIR"/opt
tar -C "$CONDA_ENV_DIR"/opt -xvzf quarto-1.9.27-linux-amd64.tar.gz
ln -s "$CONDA_ENV_DIR"/opt/quarto-1.9.27/bin/quarto "$CONDA_ENV_DIR"/bin/quarto
quarto check
quarto render run_pecan.qmd
```


## Hiccups encountered while doing the above


1. can't find `aws` on ccmmf test cluster
	- maybe part of r-bundle-bioconductor module, but not loading
	- using rclone instead, with existing config pointing to s3.garage.ccmmf.ncsa.cloud
		+ walkthrough says `aws s3 cp --endpoint-url https://s3.garage.ccmmf.ncsa.cloud s3://carb/environments/pecan-all.tar.gz ./`
		+ I used `rclone  copy ccmmf:carb/environments/pecan-all.tar.gz ./`
		+ See 2a_grass README for my rclone config

2. Not enough space for conda envs in my home directory
	- used /project/def-sponsor00/cblack/.conda instead

3. quarto not installed.
	- Will compile with rmarkdown instead. (this works for the demo on my laptop)
	- yes, works on the cluster too

4. {here} complains at load that `namespace ‘rprojroot’ 2.0.3 is being loaded, but >= 2.1.0 is required`
	- `Rscript -e 'install.packages("here")' brings in rprojroot 2.1.1
	- Did this change my Conda environment for next time? We'll find out!
	- Henry rebuilt environment, this may not affect other users.

5. In a restarted session, instructions say to use `conda activate pecan-all`, but for me this gives "bash: conda: command not found".
I'm using `source /project/def-sponsor00/cblack/.conda/envs/pecan-all/bin/activate` instead.
