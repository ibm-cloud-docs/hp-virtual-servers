---

copyright:
  years: 2019, 2020
lastupdated: "2020-10-07"

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

<!--
{:note}
When updating an image, you must provide all environment variables, including the previously set. -->

## Updating the IBM-provided image
{: #update_ibmimage}

The IBM-provided Ubuntu image is generally updated via the standard Ubuntu package manager. Use the Ubuntu image provided to replace everything within your server with the new version. The Ubuntu image resets everything to the state of a new server except for the content within `/data`.

Older instances did set the public SSH key in a way which is not compatible with the image update.
Check the information in [Updating the authorized_keys file](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-update_vs#update_authkey) to ensure that your configuration is compatible with the image update. Otherwise you will irretrievably lose access to the virtual server.  

When you update your image, the server settings are updated, for example, the [hardening configuration](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-protect_vs). The update also creates a new SSH host key (the old key is deleted).

Because there is a new SSH host key, the message below is displayed the first time to you connect after the update.
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
ibmcloud hpvs instance-update CRN
```
For more information about the update command, see [hpvs-update-instance](https://cloud.ibm.com/docs/hpvs-cli-plugin#hpvsinstanceupdate).

### Updating the authorized_keys file
{: #update_authkey}

Before you update your IBM-provided image, make sure that the root users' `authorized_keys` file is located within `/data/.ssh` and the correct file permissions are in place.

If you update your virtual server and the file is not in the correct directory, you will lose access to your virtual server forever as recovery is not possible. Instances newer than 10 September 2020 are preconfigured correctly.
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
8. Important: Don't disconnect unless you have verified that you can connect with the new setup.
9. Open a new terminal and verify that you can connect with your private key (close the new terminal afterwards and continue in the old one).
10. Optionally delete `/home/root/.ssh/authorized_keys.backup`.

## Updating your own server
{: #update_ownimage}

If you use a self-provided server, you generally don't have access to the virtual server itself (via SSH).
Updating the server enables you to update the installed packages.
The update resets everything to the state of a new server except for the content within `/data`. Before you update your server, copy any data you want to be available after the update to `/data`.
To update a self-provided server, run the following command and include the image tag (`-i mytag`).
```
ibmcloud hpvs instance-update CRN -i mytag
```
You can also update the registration definition:

```
ibmcloud hpvs instance-update CRN -i mytag --rd-path my-rd
```

Updating the registration definition does not allow you to update `repository_name`.

For more information about the update command options, see [hpvs-update-instance](https://cloud.ibm.com/docs/hpvs-cli-plugin#details_iu).

### Setting environment variables
{: #envvar_update}

Use this description only for configurations that are not considered credentials or personal data.

When you update an image, you must provide all environment variables (including the variables that you set previously).

{:note}
In this description "environment variables" are not set as environment variables, instead they provided within `/.runqenv`.

For information about environment variables, see [Configuring your server](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_config).

**Example with one environment variable:**

```
ibmcloud hpvs instance-update CRN -i latest -e name=value
```
{:pre}

**Example with multiple environment variables:**

```
ibmcloud hpvs instance-update CRN -i latest -e name1=value1 -e name2=value2`
```
{:pre}
<!--
### Setting environment variables
{: #update_own_image_environment_variables}

{:note}
The following describes how you can configure your server. Be aware that the description may only be used for configurations that are not considered credentials.

For details see [Using your own server](https://cloud.ibm.com/docs/hpvs-cli-plugin#details_iu).    the section within Provisioning > Using your own image
Environment variables need to be allowed by the registration definition, before you can set them.
You can provide environment variable by passing them via the ‘-e’ or ‘--environment’ option (cannot contain “,” at the moment).

You can allow environment variables by the name to “envs_whitelist” array within the registration definition.
Following variables cannot be set: “RUNQ_ROOTDISK”, “RUNQ_RUNQENV”, “RUNQ_SYSTEMD”, “IMAGE_TAG”, “REGION”, “PHASE”, “LPAR_NAME”, “CPC”, “RUNQ_CPU”, “RUNQ_MEM”, “POD”.

Example with one environment variable:

ibmcloud hpvs instance-update CRN -i latest -e name=value

Example with multiple environment variables:

ibmcloud hpvs instance-update CRN -i latest -e name1=value1,name2=value2

-->
