
# Obtaining and running PEcAn Demo 1

These instructions continue from the Conda environment setup in
`CARB-Demo-PEcAn-env-walkthrough.md`. They assume you're already logged in
to your compute hardware with the `pecan-all` Conda environment active.


First clone the PEcAn repo and navigate to the demo directory.

```sh
git clone https://github.com/pecanproject/pecan
cd pecan/documentation/tutorials/Demo_1_Basic_Run/
```

Work around a version complaint from the here package.
(We will fix this in the Conda environment, but it shouldn't hurt to run this
anyway).

```sh
Rscript -e 'install.packages("here")'
```

Now we're ready to compile the notebook.

```sh
Rscript -e 'rmarkdown::render("run_pecan.qmd")'
```

Hooray! This worked for me.
Verified by downloading run_pecan.html and viewing locally.


## Second rendering: With Quarto this time

Not shown live: I also tested rendering with Quarto, installed using a method
adapted from
https://quarto.org/docs/download/tarball.html?version=1.9.27&idPrefix=download-pre.
If this proves useful we can add it to the prepackaged environment or provide a
more properly Conda-approved installation guide. But in the meantime this
worked for me.

Note also that my conda envs live at a different path (because the home
partition on my cluster is small).
If your setup exactly followed Henry's walkthrough, `$CONDA_ENV_DIR` will be
`~/.conda/envs/pecan-all`.

```sh
CONDA_ENV_DIR=/project/60007/cblack/.conda/envs/pecan-all

mkdir "$CONDA_ENV_DIR"/opt
tar -C "$CONDA_ENV_DIR"/opt -xvzf quarto-1.9.27-linux-amd64.tar.gz
ln -s "$CONDA_ENV_DIR"/opt/quarto-1.9.27/bin/quarto "$CONDA_ENV_DIR"/bin/quarto
quarto check
quarto render run_pecan.qmd
```

Next week: We'll start from the same environment to run Demo 2 (uncertainty
analysis), discuss more about input setup, and get as far as we can through the
working draft of the PEPRMT demo.

