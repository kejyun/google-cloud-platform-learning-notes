# gcloud

## 安裝

至[從版本化封存檔中安裝](https://cloud.google.com/sdk/docs/downloads-versioned-archives)頁面，下載和您的版本相容的適當封存檔，將下載的 `google-cloud-sdk-208.0.1-darwin-x86_64.tar.gz` 解壓縮至家目錄

```
~/google-cloud-sdk/install.sh
Welcome to the Google Cloud SDK!

To help improve the quality of this product, we collect anonymized usage data
and anonymized stacktraces when crashes are encountered; additional information
is available at <https://cloud.google.com/sdk/usage-statistics>. You may choose
to opt out of this collection now (by choosing 'N' at the below prompt), or at
any time in the future by running the following command:

    gcloud config set disable_usage_reporting true

Do you want to help improve the Google Cloud SDK (Y/n)?  n
Your current Cloud SDK version is: 208.0.1
The latest available version is: 224.0.0

┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                   Components                                                   │
├──────────────────┬──────────────────────────────────────────────────────┬──────────────────────────┬───────────┤
│      Status      │                         Name                         │            ID            │    Size   │
├──────────────────┼──────────────────────────────────────────────────────┼──────────────────────────┼───────────┤
│ Update Available │ BigQuery Command Line Tool                           │ bq                       │   < 1 MiB │
│ Update Available │ Cloud SDK Core Libraries                             │ core                     │   8.8 MiB │
│ Update Available │ Cloud Storage Command Line Tool                      │ gsutil                   │   3.5 MiB │
│ Not Installed    │ App Engine Go Extensions                             │ app-engine-go            │  56.4 MiB │
│ Not Installed    │ Cloud Bigtable Command Line Tool                     │ cbt                      │   6.3 MiB │
│ Not Installed    │ Cloud Bigtable Emulator                              │ bigtable                 │   4.3 MiB │
│ Not Installed    │ Cloud Datalab Command Line Tool                      │ datalab                  │   < 1 MiB │
│ Not Installed    │ Cloud Datastore Emulator                             │ cloud-datastore-emulator │  17.7 MiB │
│ Not Installed    │ Cloud Datastore Emulator (Legacy)                    │ gcd-emulator             │  38.1 MiB │
│ Not Installed    │ Cloud Pub/Sub Emulator                               │ pubsub-emulator          │  33.4 MiB │
│ Not Installed    │ Cloud SQL Proxy                                      │ cloud_sql_proxy          │   3.7 MiB │
│ Not Installed    │ Emulator Reverse Proxy                               │ emulator-reverse-proxy   │  14.5 MiB │
│ Not Installed    │ Google Cloud Build Local Builder                     │ cloud-build-local        │   5.9 MiB │
│ Not Installed    │ Google Container Registry's Docker credential helper │ docker-credential-gcr    │   1.8 MiB │
│ Not Installed    │ gcloud Alpha Commands                                │ alpha                    │   < 1 MiB │
│ Not Installed    │ gcloud Beta Commands                                 │ beta                     │   < 1 MiB │
│ Not Installed    │ gcloud app Java Extensions                           │ app-engine-java          │ 108.8 MiB │
│ Not Installed    │ gcloud app PHP Extensions                            │ app-engine-php           │  21.9 MiB │
│ Not Installed    │ gcloud app Python Extensions                         │ app-engine-python        │   6.2 MiB │
│ Not Installed    │ gcloud app Python Extensions (Extra Libraries)       │ app-engine-python-extras │  28.5 MiB │
│ Not Installed    │ kubectl                                              │ kubectl                  │   < 1 MiB │
└──────────────────┴──────────────────────────────────────────────────────┴──────────────────────────┴───────────┘
To install or remove components at your current SDK version [208.0.1], run:
  $ gcloud components install COMPONENT_ID
  $ gcloud components remove COMPONENT_ID

To update your SDK installation to the latest version [224.0.0], run:
  $ gcloud components update


Modify profile to update your $PATH and enable shell command
completion?

Do you want to continue (Y/n)? Y

The Google Cloud SDK installer will now prompt you to update an rc
file to bring the Google Cloud CLIs into your environment.

Enter a path to an rc file to update, or leave blank to use
[/Users/kejyun/.bash_profile]:
Backing up [/Users/kejyun/.bash_profile] to [/Users/kejyun/.bash_profile.backup].
[/Users/kejyun/.bash_profile] has been updated.

==> Start a new shell for the changes to take effect.


For more information on how to get started, please visit:
  https://cloud.google.com/sdk/docs/quickstarts
```

## 初始化 gcloud 設定

```
gcloud init
```

輸入指令後，初始化需要連線到 GCP 主機的相關區域及連線設定，會開啟瀏覽器取得 google 帳號的授權


```
$ gcloud init
Welcome! This command will take you through the configuration of gcloud.

Settings from your current configuration [default] are:
compute:
  region: asia-east1
  zone: asia-east1-a
core:
  account: kj@kejyun.com
  disable_usage_reporting: 'False'
  project: kejyun-corp

Pick configuration to use:
 [1] Re-initialize this configuration [default] with new settings
 [2] Create a new configuration
Please enter your numeric choice:  2

Enter configuration name. Names start with a lower case letter and
contain only lower case letters a-z, digits 0-9, and hyphens '-':  kejyun-research
Your current configuration has been set to: [kejyun-research]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.
Reachability Check passed.
Network diagnostic (1/1 checks) passed.

Choose the account you would like to use to perform operations for
this configuration:
 [1] kj@kejyun.com
 [2] Log in with a new account
Please enter your numeric choice:  2

Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?XXXXXXX


Updates are available for some Cloud SDK components.  To install them,
please run:
  $ gcloud components update

You are logged in as: [kj@kejyun.com].

Pick cloud project to use:
 [1] kejyun-analytics-167805
 [2] kejyun-research
 [3] Create a new project
Please enter numeric choice or text value (must exactly match list
item):  2

Your current project has been set to: [kejyun-research].

Do you want to configure a default Compute Region and Zone? (Y/n)?  Y

Which Google Compute Engine zone would you like to use as project
default?
If you do not specify a zone via a command line flag while working
with Compute Engine resources, the default is assumed.
 [1] us-east1-b
 [2] us-east1-c
 [3] us-east1-d
 [4] us-east4-c
 [5] us-east4-b
 [6] us-east4-a
 [7] us-central1-c
 [8] us-central1-a
 [9] us-central1-f
 [10] us-central1-b
 [11] us-west1-b
 [12] us-west1-c
 [13] us-west1-a
 [14] europe-west4-a
 [15] europe-west4-b
 [16] europe-west4-c
 [17] europe-west1-b
 [18] europe-west1-d
 [19] europe-west1-c
 [20] europe-west3-b
 [21] europe-west3-c
 [22] europe-west3-a
 [23] europe-west2-c
 [24] europe-west2-b
 [25] europe-west2-a
 [26] asia-east1-b
 [27] asia-east1-a
 [28] asia-east1-c
 [29] asia-southeast1-b
 [30] asia-southeast1-a
 [31] asia-southeast1-c
 [32] asia-northeast1-b
 [33] asia-northeast1-c
 [34] asia-northeast1-a
 [35] asia-south1-c
 [36] asia-south1-b
 [37] asia-south1-a
 [38] australia-southeast1-b
 [39] australia-southeast1-c
 [40] australia-southeast1-a
 [41] southamerica-east1-b
 [42] southamerica-east1-c
 [43] southamerica-east1-a
 [44] asia-east2-a
 [45] asia-east2-b
 [46] asia-east2-c
 [47] europe-north1-a
 [48] europe-north1-b
 [49] europe-north1-c
 [50] northamerica-northeast1-a
Did not print [6] options.
Too many options [56]. Enter "list" at prompt to print choices fully.
Please enter numeric choice or text value (must exactly match list
item):  26

Your project default Compute Engine zone has been set to [asia-east1-b].
You can change it by running [gcloud config set compute/zone NAME].

Your project default Compute Engine region has been set to [asia-east1].
You can change it by running [gcloud config set compute/region NAME].

Your Google Cloud SDK is configured and ready to use!

* Commands that require authentication will use kj@kejyun.com by default
* Commands will reference project `kejyun-research` by default
* Compute Engine commands will use region `asia-east1` by default
* Compute Engine commands will use zone `asia-east1-b` by default

Run `gcloud help config` to learn how to change individual settings

This gcloud configuration is called [kejyun-research]. You can create additional configurations if you work with multiple accounts and/or projects.
Run `gcloud topic configurations` to learn more.

Some things to try next:

* Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
* Run `gcloud topic -h` to learn about advanced features of the SDK like arg files and output formatting
```


## 使用 gcloud 連線到 Compute Engine 主機

```
gcloud compute --project "<project-name>" ssh --zone <backend-zone> <backend-name>
```

```
gcloud compute --project "kejyun-research" ssh --zone asia-east1-b kejyun-dev
```


```
gcloud compute --project "kejyun-research" ssh --zone asia-east1-b kejyun-dev
Updating project ssh metadata...\Updated [https://www.googleapis.com/compute/v1/projects/kejyun-research].
Updating project ssh metadata...done.
Waiting for SSH key to propagate.
Warning: Permanently added 'compute.3581355771881432819' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1023-gcp x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud
This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

0 packages can be updated.
0 updates are security updates.

kejyun@kejyun-dev:~$
```


## 參考資料
* [安裝 Google Cloud SDK  |  Cloud SDK 說明文件  |  Google Cloud](https://cloud.google.com/sdk/install)
* [從版本化封存檔中安裝  |  Cloud SDK 說明文件  |  Google Cloud](https://cloud.google.com/sdk/docs/downloads-versioned-archives)
