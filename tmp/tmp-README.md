

```
gcloud compute --project \
    "tnl-research" ssh --zone \
    <backend-zone> <backend-name>
```


```
gcloud compute --project \
    "tnl-research" ssh --zone \
    us-central1-c mongo
```

```
gcloud compute --project "tnl-research" ssh --zone asia-east1-b inside-dev
```


```
WARNING: The public SSH key file for gcloud does not exist.
WARNING: The private SSH key file for gcloud does not exist.
WARNING: You do not have an SSH key for gcloud.
WARNING: SSH keygen will be executed to generate a key.
This tool needs to create the directory [/home/kejyun/.ssh] before
being able to generate SSH keys.
Do you want to continue (Y/n)? Y

The key fingerprint is:
SHA256:8f67SxHHbfVGbB1mOrlWwAQawmS5jxAxxJB2aIrcSYg kejyun@cs-6000-devshell-vm-b0b5a53c-2000-4568-97c3-f3273bdb8aef
The key's randomart image is:
+---[RSA 2048]----+
|. ..*+++.. .+o ==|
|E. * +oo. o  o*o*|
|o * o . o.  .+oo=|
|.o o . . o   o+o |
|      . S . .o   |
|       . o  ..   |
|          . .    |
|           o     |
|            =+   |
+----[SHA256]-----+
Updating project ssh metadata
Updated [https://www.googleapis.com/compute/v1/projects/tnl-research].
Updating project ssh metadata...done.
Waiting for SSH key to propagate.
Warning: Permanently added 'compute.4861259702581439659' (ECDSA) to the list of known hosts.
Graph this data and manage this system at:
  https://landscape.canonical.com/
Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud
0 packages can be updated.
0 updates are security updates.
New release '16.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
Your Hardware Enablement Stack (HWE) is supported until April 2019.
kejyun@mongo:~$
```


```
sudo apt-get update
```

```
sudo apt-get install mongodb
```


```
sudo service mongodb stop
```

## Frontend Mongo

```
gcloud compute --project \
    "tnl-research" ssh --zone \
    us-central1-c		 frontend-mongo
```

```
sudo apt-get update
```

```
curl -sL \
    https://deb.nodesource.com/setup_6.x \
    | sudo -E bash -
```

```
sudo apt-get install git nodejs
```

```
git clone \
    https://github.com/GoogleCloudPlatform/todomvc-mongodb.git
```

```
cd todomvc-mongodb; npm install
```

```
sed -i -e 's/8080/80/g' server.js
```

```
sudo nohup nodejs server.js --be_ip \
    <backend-internal-ip> --fe_ip \
    <frontend-internal-ip> &
```


```
sudo nohup nodejs server.js --be_ip \
    10.128.0.2 --fe_ip \
    10.128.0.3 &
```
