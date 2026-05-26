# S3 access configuration for MAGiC file retrieval

This guide shows how to configure your access keys to retrieve data from the project fileserver, which is hosted by project partner NCSA and speaks the S3 protocol. We use the `aws` command line tools to interact with that server, but importantly this is _not_ an Amazon service; if you ever see an error messages that includes a URL with "amazonaws.com" in it, that's a good sign something is not configured right.

To configure S3 access you need five pieces of information:
1. A _profile name_: `ccmmf`
2. An _access key id_: Provided separately. Looks like 26 case-sensitive hexadecimal digits.
3. A _secret access key_: Provided separately. Looks like 64 case-sensitive hexadecimal digits.
4. A _region name_: `garage`
5. An _endpoint url_: `https://garage.s3.ccmmf.ncsa.cloud`

If you have any existing AWS configurations for other projects, the steps below will leave them untouched. When looking at the configuration files afterward, you should see a `ccmmf` section added to the end with your pre-existing profile(s) still in place above it.


## Set access keys

```sh
aws configure --profile ccmmf
```

It will prompt you for your access key id, then secret access key. Paste in each one and press enter to submit. It will then prompt for a default region name. Type `garage` and press enter. Last will be a default output format. Press enter to leave this unset.

This will write the keys to a file named `~/.aws/credentials`. If everything went well, the last (or only, if this is your first time configuring AWS on this machine) section of that file should now look like the following:

```
[ccmmf]
aws_access_key_id = your24digitkeyidhere
aws_secret_access_key = 64hexdigitshere
```


## Set endpoint URL

Now set the endpoint url by adding it to your configuration file. `aws configure` does not set this so we have to manually edit the file.

* Safer option: In your editor of choice, open `~/.aws/config` and find the line that reads `[profile ccmmf]`. Immediately below that, add a line that reads `endpoint_url = https://s3.garage.ccmmf.ncsa.cloud`.
* YOLO option: If you are _sure_ the `ccmmf` profile is at the bottom of your config file so that adding a new line to the end of the file is adding it to the `ccmmf` profile, skip opening your editor and do `echo 'endpoint_url = https://s3.garage.ccmmf.ncsa.cloud' >> ~/.aws/configure`

If everything went well, the last (or only) section of `~/.aws/config` should look like the following:

```
[profile ccmmf]
region = garage
endpoint_url = https://s3.garage.ccmmf.ncsa.cloud
```

Note also how the section header for the config file is `[profile ccmmf]` while the credential file just uses `[ccmmf]` without the word "profile". Make sure not to confuse these when hand-editing the files, or strange errors will happen... ask us how we know this :(

## Set profile in your environment

When you are working inside the MAGiC conda environment, these are set for you, but for first-time setup you need to do it by hand.

```sh
export AWS_PROFILE=ccmmf
```

## Test your access

```sh
aws s3 ls carb/
```

If you see a list of directory names that includes `data/`, `data_raw/`, and `deploy/` among others, it's working. 

TK here: Commonly seen errors and fixes for them.


## Alternate configurations

This guide tries to provide one method that should work for everyone whether or not they need to connect to other AWS services. However the aws toolkit is complex and highly flexible, so there's nearly always another way to do it. Feel free to explore and see what works for you.

One trick that's good to know for debugging is that you can override any of the config file values we showed here by setting environment variables (`AWS_ENDPOINT_URL`, `AWS_ACCESS_KEY_ID`, etc.), and you can further override many of those by adding flags to an individual command (`--profile`, `--endpoint-url`, etc). So for example if `aws s3 ls` fails but `aws s3 ls --endpoint_url https://s3.garage.ccmmf.ncsa.cloud` succeeds, you know your access keys are working and you're "just" debugging the URL configuration.
