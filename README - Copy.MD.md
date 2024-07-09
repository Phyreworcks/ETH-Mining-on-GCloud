\# GCP Ethereum (ETH) and Ethereum Classic (ETC) Miner

Terraform template for mining \_Ethereum (ETH)\_ and \_Ethereum Classic (ETC)\_ crypto currencies on Google

Cloud Platform (GCP) GPU-enabled VM instances.

<img align="centre" src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Google\_Cloud\_logo.svg/320px-Google\_Cloud\_logo.svg.png"/>

<img align="right" src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Ethereum\_logo\_2014.svg/128px-Ethereum\_logo\_2014.svg.png"/>

\## Important!

* GCP is very sensitive about crypto mining on their platform. The scripts and

templates presented here have been carefully crafted to bypass detection however

as always \_run it at your own risk\_.

* Even more importantly: \_don't change the startup script\_ or you may expose yourself

to the GCP wrath.

* Ethereum mining may not be profitable at all times and it can get \_very expensive

very quickly\_ - you have been warned! Unless you've got access to a free GCP account

it may not be the wisest thing to do!

\## Medium article

Check out my blog post [\*\*Easy Etherum mining on GCP\*\*](https://medium.com/coinmonks/easy-ethereum-mining-on-gcp-576f0aaaeeed) for more details.

\## Quick start

The easiest way to start is by using the [Google Cloud Shell](https://cloud.google.com/shell).

I assume that you already have a \_GCP Project\_ set up, let's say it's called `mylatestproject9`.

1. Login to your [Google Cloud Console](https://console.cloud.google.com/)

1. Open the cloud shell using the icon in the top-right corner of the console.

1. Once the shell starts up you may have to select the right project first:

cloudshell:~$ gcloud config set project mylatestproject9

Updated property [core/project].

cloudshell:~ (mylatestproject9)$

1. Next we clone this GIT repository to Cloud Shell:

cloudshell:~ (mylatestproject9)$ git clone https://github.com/mludvig/gcp-ethereum-miner.git

Cloning into 'gcp-ethereum-miner'...

remote: Enumerating objects: 41, done.

remote: Counting objects: 100% (41/41), done.

remote: Compressing objects: 100% (20/20), done.

remote: Total 41 (delta 23), reused 39 (delta 21), pack-reused 0

Receiving objects: 100% (41/41), 5.80 KiB | 2.90 MiB/s, done.

Resolving deltas: 100% (23/23), done.

cloudshell:~ (mylatestproject9)$ cd gcp-ethereum-miner

cloudshell:~/gcp-ethereum-miner (mylatestproject9)$

1. Configure `terraform.tfvars` to your liking.

\# Ethereum (ETH) or Ethereum Classic (ETC)

coin\_name           = "ETC"

\# Ethereum wallet address

wallet\_address      = "0x0829Ac56e33E615ADBC5977FF1A49aF508Ed0499"

\# Launch instances in these provisioning models (best for high availability)

provisioning\_models = ["SPOT", "STANDARD"]

\# GPU types to use

gpu\_types           = ["t4", "a100", "v100"]

\# Number of instances per-gpu / per-region / per-provisioning\_model

group\_size          = 16

A note about `group\_size` - each combination of GPU type + Region + Spot/Standard mode creates

a separate \_Instance Group\_ and each instance group will have the configured `group\_size`.

It means that the number of instances eventually running will be up to \*group\_size \*

number of instance groups\*, of course only if your Service Quotas permit.

1. Now's the time to start mining!

cloudshell:~/gcp-ethereum-miner (mylatestproject9)$ terraform init

cloudshell:~/gcp-ethereum-miner (mylatestproject9)$ terraform apply -auto-approve

You may be presented with a pop up saying "Authorize Cloud Shell" to make GCP API calls - click \*Authorize\*.

You will see how Terraform starts creating the Instance Templates and Instance Groups.

When it's done head over to the [VM console](https://console.cloud.google.com/compute/instances)

and you should see some instances starting up.

\## GPU Quotas

If there are none check out the [Instance Groups](https://console.cloud.google.com/compute/instanceGroups/list)

console and in there the ERRORS tab in some of the IGs to figure out what's going on.

Most likely you'll be limited by \*\*GPU Quotas\*\* and will see messages like:

* Instance creation failed: Quota GPUS\_ALL\_REGIONS exceeded. Limit 0.0 globally.
* Instance creation failed: Quota PREEMTIBLE\_NVIDIA\_T4\_GPUS exceeded. Limit 0.0 in region us-central1.

If that's the case head to \*IAM -> Quotas\*, search for the above (e.g. GPUS\_ALL\_REGIONS) and try to get them increased.

The form will ask for a reason or justification. Needless to say that "Crypto mining" is not a good one to put forward :)

\## Happy mining!
