---

copyright:
  years: 2019, 2024
lastupdated: "2025-04-03"

subcollection: hp-virtual-servers

keywords: update, image
---


{{site.data.keyword.attribute-definition-list}}

# Updating a virtual server
{: #update_vs}

{{site.data.keyword.hpvs}} is deprecated. As of 18 August 2024, you can’t create new instances, and access to free instances will be removed. Existing premium plan instances are supported until 31 January 2025. Any instances that still exist on that date will be deleted.
{: deprecated}

You can update the OCI Image that is used for your Hyper Protect virtual server.

To trigger an update, you must have the IAM editor role (platform access) on the server.
You can only update the server by way of the {{site.data.keyword.hpvs}} [CLI](/docs/hp-virtual-servers?topic=hp-virtual-servers-hpvs_cli_plugin).
Before you update a server, back up all your data and make sure you can restore the data from your backups. IBM can't restore any data for you.


## Updating the IBM-provided image
{: #update_ibmimage}

The IBM-provided Ubuntu image is generally updated thru the standard Ubuntu package manager. Use the Ubuntu image provided to replace everything within your server with the new version. The Ubuntu image resets everything to the state of a new server except for the content within `/data`.

Older instances did set the public SSH key in a way, which is not compatible with the image update.
Check the information in [Updating the authorized_keys file](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-update_vs#update_authkey) to ensure that your configuration is compatible with the image update. Otherwise, you  irretrievably lose access to the virtual server.

When you update your image, the server settings are updated, for example, the [hardening configuration](/docs/hp-virtual-servers?topic=hp-virtual-servers-protect_vs). The update also creates a new SSH host key (the old key is deleted).

The following message is displayed the first time to you connect after the update because there is a new SSH host key.
```sh
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
{: screen}


To update your image, run:
```sh
ibmcloud hpvs instance-update (NAME | CRN)
```
{: pre}

For information about the update command, see [hpvs-update-instance](/docs/hp-virtual-servers?topic=hp-virtual-servers-hpvs_cli_plugin#hpvsinstanceupdate).

### Updating the authorized_keys file
{: #update_authkey}

Before you update your IBM-provided image, make sure that the root users' `authorized_keys` file is located within `/data/.ssh` and the correct file permissions are in place.

If you update your virtual server and the file is not in the correct directory, you lose access to your virtual server forever as recovery is not possible. Instances newer than 10 September 2020 are preconfigured correctly.
{: note}

To place the `authorized_keys` file images in the `data/.ssh` directory, complete the following steps:

1. Create the directory .ssh within `/data`:
   ```sh
   mkdir /data/.ssh
   ```
   {: pre}

2. Copy the `authorized_keys` file:
   ```sh
   cp /home/root/.ssh/authorized_keys /data/.ssh/authorized_keys
   ```
   {: pre}

3. Make sure the owner of `/data/.ssh` and `/data/.ssh` is `root:root`.
4. Make sure the file permissions for `/data.ssh` are `drwxr-xr-x`.
5. Make sure the file permissions for `/data/.ssh/authorized_keys` are `-rw-r--r--`.
6. Rename the original file:
   ```sh
   mv /home/root/.ssh/authorized_keys /home/root/.ssh/authorized_keys.backup
   ```
   {: pre}

7. Create a symlink as the one within the current image:
   ```sh
   ln -s /data/.ssh/authorized_keys /home/root/.ssh/authorized_keys
   ```
   {: pre}

8. Important: Do not disconnect unless you verified that you can connect with the new setup.
9. Open a new terminal and verify that you can connect with your private key (close the new terminal afterward and continue in the old one).
10. Optionally delete `/home/root/.ssh/authorized_keys.backup`.

## Updating your own server
{: #update_ownimage}

If you use a self-provided server, you generally don't have access to the virtual server itself (thru SSH).
When you update the server you can update the installed packages.
The update resets everything to the state of a new server except for the content within `/data`. Before you update your server, copy any data that you want to be available after the update to `/data`.
To update a self-provided server, run the following command and include the image tag (`-i mytag`).
```sh
ibmcloud hpvs instance-update (NAME | CRN) -i mytag
```
{: pre}

You can also update the registration definition:

```sh
ibmcloud hpvs instance-update (NAME | CRN) -i mytag --rd-path my-rd
```
{: pre}

An update the registration definition does not allow you to update `repository_name`.

For information about the update command options, see [hpvs-update-instance](/docs/hp-virtual-servers?topic=hp-virtual-servers-hpvs_cli_plugin#details_iu).

### Setting environment variables
{: #envvar_update}

Use this description for configurations that are not considered credentials or personal data only.

When you update an image, you must provide all environment variables (including the variables that you set previously).


In this description "environment variables" are not set as environment variables, instead they provided within `/.runqenv`.
For information about environment variables, see [Configuring your server](/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi#byoi_config).
{: note}


**Example with one environment variable:**

```sh
ibmcloud hpvs instance-update (NAME | CRN) -i latest -e name=value
```
{: pre}

**Example with multiple environment variables:**

```sh
ibmcloud hpvs instance-update (NAME | CRN) -i latest -e name1=value1 -e name2=value2`
```
{: pre}
