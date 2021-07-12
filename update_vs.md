---

copyright:
  years: 2019, 2020, 2021
lastupdated: "2021-03-19"

subcollection: hp-virtual-servers

keywords: update, image
---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Updating a virtual server
{: #update_vs}

You can update the OCI Image that is used for your Hyper Protect virtual server.

To trigger an update, you must have the IAM editor role (platform access) on the server.
You can only update the server by way of the {{site.data.keyword.hpvs}} [CLI](https://cloud.ibm.com/docs/hpvs-cli-plugin).
Before you update a server, back up all your data and make sure you can restore the data from your backups. IBM can't restore any data for you.


## Updating the IBM-provided image
{: #update_ibmimage}

The IBM-provided Ubuntu image is generally updated thru the standard Ubuntu package manager. Use the Ubuntu image provided to replace everything within your server with the new version. The Ubuntu image resets everything to the state of a new server except for the content within `/data`.

Older instances did set the public SSH key in a way, which is not compatible with the image update.
Check the information in [Updating the authorized_keys file](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-update_vs#update_authkey) to ensure that your configuration is compatible with the image update. Otherwise, you  irretrievably lose access to the virtual server.  

When you update your image, the server settings are updated, for example, the [hardening configuration](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-protect_vs). The update also creates a new SSH host key (the old key is deleted).

The following message is displayed the first time to you connect after the update because there is a new SSH host key.
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:+AuFsuEHkEpIZs0Z2XSvrEPDXH4bKJWVoKhD9gZOWm8.
Please contact your system administrator.
Add correct host key in /Users/username/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/username/.ssh/known_hosts:5
ECDSA host key for 52.116.29.107 has changed and you have requested strict checking.
Host key verification failed.
```
{:note}

To update your image, run:
```
ibmcloud hpvs instance-update (NAME | CRN)
```
For information about the update command, see [hpvs-update-instance](https://cloud.ibm.com/docs/hpvs-cli-plugin#hpvsinstanceupdate).

### Updating the authorized_keys file
{: #update_authkey}

Before you update your IBM-provided image, make sure that the root users' `authorized_keys` file is located within `/data/.ssh` and the correct file permissions are in place.

If you update your virtual server and the file is not in the correct directory, you lose access to your virtual server forever as recovery is not possible. Instances newer than 10 September 2020 are preconfigured correctly.
{: note}

To place the `authorized_keys` file images in the `data/.ssh` directory, complete the following steps:

1. Create the directory .ssh within `/data`:
```
mkdir /data/.ssh
```
2. Copy the `authorized_keys` file:
```
cp /home/root/.ssh/authorized_keys /data/.ssh/authorized_keys
```
3. Make sure the owner of `/data/.ssh` and `/data/.ssh` is `root:root`.
4. Make sure the file permissions for `/data.ssh` are `drwxr-xr-x`.
5. Make sure the file permissions for `/data/.ssh/authorized_keys` are `-rw-r--r--`.
6. Rename the original file:
```
mv /home/root/.ssh/authorized_keys /home/root/.ssh/authorized_keys.backup
```
7. Create a symlink as the one within the current image:
```
ln -s /data/.ssh/authorized_keys /home/root/.ssh/authorized_keys
```
8. Important: Don't disconnect unless you verified that you can connect with the new setup.
9. Open a new terminal and verify that you can connect with your private key (close the new terminal afterward and continue in the old one).
10. Optionally delete `/home/root/.ssh/authorized_keys.backup`.

## Updating your own server
{: #update_ownimage}

If you use a self-provided server, you generally don't have access to the virtual server itself (thru SSH).
When you update the server you can update the installed packages.
The update resets everything to the state of a new server except for the content within `/data`. Before you update your server, copy any data that you want to be available after the update to `/data`.
To update a self-provided server, run the following command and include the image tag (`-i mytag`).
```
ibmcloud hpvs instance-update (NAME | CRN) -i mytag
```
You can also update the registration definition:

```
ibmcloud hpvs instance-update (NAME | CRN) -i mytag --rd-path my-rd
```

An update the registration definition does not allow you to update `repository_name`.

For information about the update command options, see [hpvs-update-instance](https://cloud.ibm.com/docs/hpvs-cli-plugin#details_iu).

### Setting environment variables
{: #envvar_update}

Use this description for configurations that are not considered credentials or personal data only.

When you update an image, you must provide all environment variables (including the variables that you set previously).

{:note}
In this description "environment variables" are not set as environment variables, instead they provided within `/.runqenv`.
For information about environment variables, see [Configuring your server](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_config).

**Example with one environment variable:**

```
ibmcloud hpvs instance-update (NAME | CRN) -i latest -e name=value
```
{:pre}

**Example with multiple environment variables:**

```
ibmcloud hpvs instance-update (NAME | CRN) -i latest -e name1=value1 -e name2=value2`
```
{:pre}
