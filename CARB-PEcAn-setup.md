# CARB PEcAn Environment Setup

This guide covers configuring S3 access and installing the PEcAn conda environment needed to run the CARB pipeline.

The environment is downloaded from a project fileserver hosted by NCSA that speaks the S3 protocol. This is _not_ an Amazon service — if you ever see an error message containing `amazonaws.com`, something is misconfigured.

Note that the full install may take 30 minutes or more.

Note: do not install this environment to the same location of an existing environment unless you remove the existing one first.

## Dependencies

### Conda
A conda distribution (e.g. miniconda) must be on your path. If it is not present, you can follow this guide [to install miniconda](https://www.anaconda.com/docs/getting-started/miniconda/install/overview).

### AWS CLI
The AWS CLI is used to download files from the project fileserver. You will need five pieces of information to configure it:

1. **Profile name**: `ccmmf`
2. **Region name**: `garage`
3. **Endpoint URL**: `https://s3.garage.ccmmf.ncsa.cloud`
4. **Access key ID**: provided separately — looks like 26 case-sensitive hexadecimal digits
5. **Secret access key**: provided separately — looks like 64 case-sensitive hexadecimal digits

## AWS CLI Profile Setup

Using a named profile keeps the CCMMF credentials and endpoint isolated from any other AWS configuration you may have, preventing conflicts.

### Set credentials

Run the following and paste in your access key ID and secret access key when prompted. When asked for a default region, enter `garage`. When asked for output format, press enter to accept the default.

```bash
aws configure --profile ccmmf
```

This writes your credentials to `~/.aws/credentials`. The resulting `ccmmf` section should look like:

```ini
[ccmmf]
aws_access_key_id = your24digitkeyidhere
aws_secret_access_key = yoursecretgoeshereitshouldbe64digitslong
```

### Set endpoint URL

`aws configure` does not set the endpoint URL, so add it manually.

If you're sure the `ccmmf` section is the last one in your config file (which it will be if doing this immediately after adding it via `aws configure` or if you don't have any other profiles):

```bash
`echo 'endpoint_url = https://s3.garage.ccmmf.ncsa.cloud' >> ~/.aws/config`
```

Or if you're not sure: Open `~/.aws/config` in your text editor, find the section labeled `[ccmmf]`, and add a line immediately below it that reads `endpoint_url = https://s3.garage.ccmmf.ncsa.cloud`.


The resulting `ccmmf` section of `~/.aws/config` should look like:

```ini
[profile ccmmf]
region = garage
endpoint_url = https://s3.garage.ccmmf.ncsa.cloud
```

Note: `region = garage` is required by the AWS CLI to construct certain requests correctly, even though this is not an AWS service. Also note that the config file uses `[profile ccmmf]` while the credentials file uses `[ccmmf]` — the word `profile` is only present in the config file. Hand-editing errors here cause difficult-to-diagnose failures.

### Verify access

Run either of the following. If the profile is configured correctly, you will see a listing of the CARB bucket contents.

```bash
aws s3 ls --profile ccmmf s3://carb/
```

```bash
AWS_PROFILE=ccmmf aws s3 ls s3://carb/
```

## Install

In this example, `~/.conda/envs/pecan-all` is the target location, but you may want to put this somewhere other than your home directory.

The version specified here (`1.10`) is the only version currently available, but this will change over time. Choose a target location with the expectation that you will need to install a new version in the future.

### With export

Exporting `AWS_PROFILE` once for the session means all subsequent commands pick it up automatically, including the setup script:

```bash
export AWS_PROFILE=ccmmf
aws s3 cp s3://carb/deploy/setup-pecan-env.sh ./
bash setup-pecan-env.sh 1.10 ~/.conda/envs/pecan-all
```

### Without export

If you prefer not to export, pass the profile explicitly on each command:

```bash
aws s3 cp --profile ccmmf s3://carb/deploy/setup-pecan-env.sh ./
AWS_PROFILE=ccmmf bash setup-pecan-env.sh 1.10 ~/.conda/envs/pecan-all
```

### Activate the environment

To activate the pecan-all environment in a new shell:

```bash
conda activate ~/.conda/envs/pecan-all
```

## What's Happening in the Background

The setup script downloads a tarball from S3 and extracts it to the specified location. The tarball contains a conda environment with the necessary system libraries; the conda environment is unpacked via conda-unpack and the Python and system libraries are configured for your local environment.

The R libraries are not managed by conda. A lockfile is included within the tarball that specifies the exact versions of the R packages to install. This is the portion that can take the most time on first install. If you reinstall the environment in a new location, cached versions of the R packages will be used from your previous install, saving time.

## Troubleshooting

The AWS CLI has a strict override order: config file values can be overridden by environment variables (`AWS_ENDPOINT_URL`, `AWS_ACCESS_KEY_ID`, etc.), which can be further overridden by per-command flags (`--profile`, `--endpoint-url`, etc.). This is useful for debugging. For example, if `aws s3 ls` fails but `aws s3 ls --endpoint-url https://s3.garage.ccmmf.ncsa.cloud` succeeds, your credentials are working and the issue is with the endpoint configuration.

If you see any error message containing `amazonaws.com`, the CCMMF endpoint is not being used — check your profile config.

## Contact
Please reach out to hdpriest@illinois.edu if you have any issues with the install process.
