---

copyright:
  years: 2020, 2023
lastupdated: "2022-02-14"

keywords: commands, cluster resource, hpvs-cli plugin, hpvs CLI, hpvs-cli command line , hpvs-cli shell


subcollection: hp-virtual-servers

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} CLI
{: #hpvs_cli_plugin}

Use the {{site.data.keyword.hpvs}} CLI to list, create, or delete your {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} instances.
{: shortdesc}

The {{site.data.keyword.hpvs}} CLI is available for the amd64 architecture and IBM Z platform (s390x architecture) on Linux&reg;, amd64 architecture on Windows&reg;, and Mac.

## Prerequisites
{: #prerequisites_hpvs_cli_plugin}


- Install the [{{site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started). The prefix for running commands by using the {{site.data.keyword.cloud_notm}} CLI is `ibmcloud`.

- Install the {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} CLI by running the following command:

   ```sh
   ibmcloud plugin install hpvs
   ```
   {: pre}

If you want to view the current version of your {{site.data.keyword.hpvs}}
CLI plug-in, run `ibmcloud plugin show hpvs`.

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and [plug-ins](https://cloud.ibm.com/docs/cli?topic=cli-plug-ins) are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: tip}

{{site.data.keyword.cloud_notm}} CLI requires Java&reg; 1.8.0. You can download the CLI from {{site.data.keyword.cloud_notm}} to use on your local system as a complement to the {{site.data.keyword.cloud_notm}} console.
{: note}

Do not use personal information, for example, your name, as the instance name or as part of the instance name. The data that you provide when you provision an instance or interact with the hpvs cli is not considered to be personal data or credentials.
Learn more about IBM Cloud Hyper Protect Virtual Servers' Data usage and Certifications [here](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=165C6EF0FFDA11E8BABD512A6952EE1F).
{: note}

## CLI commands
{: #plugin_use}

## hpvs help
{: #display_list}

This command displays a list of hpvs commands.

```sh
ibmcloud hpvs help
```
{: pre}


## hpvs instances
{: #instances_list}

This command lists all your {{site.data.keyword.hpvs}} instances.

```sh
ibmcloud hpvs instances [--output json]
```
{: pre}

### Command options
{: #instances_co}

`--output`
:   Displays results in the requested format. The only valid value is `json`.



## hpvs instance
{: #details_list}

This command shows details about the server instance.

```sh
ibmcloud hpvs instance (NAME | CRN) [--output json]
```
{: pre}



`NAME`
:   The name of your new instance. An error message is displayed if you specify a NAME that is not unique, for example,

```sh
VS name 'ABC' is ambiguous: more than one VS exist for the provided name: [crn1 crn2].
```
{: codeblock}

Specify the `CRN` if your instance name is not unique.

`CRN`
:   The server's Cloud resource name (CRN). Specify the `CRN` if `NAME` is not unique. You can run the `ibmcloud hpvs instances` command to get the CRN.


### Command options
{: #details_co}


`--output`
:   Displays results in the requested format. The only valid value is `json`.


### Example output
{: #details_eo}

```json
Name                  hpvs-env-test
CRN                   crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:0b2df6e9-ec2c-4b4a-87dd-60f53f6a2a0d::
Location              dal13
Cloud tags
Cloud state           active
Server status         running
Plan                  Free
Public IP address     52.116.29.73
Internal IP address   172.17.151.218
Storage               50 GiB
Memory                2048 MiB
Processors            1 vCPUs
Image type            self-provided
Image OS              self-defined
Image name            de.icr.io/hpvs_test/ubuntu:v2
Environment           TEST_VAR=value
                      TEST_VAR2=value2
Last operation        update succeeded
```
{: codeblock}

### Example JSON output during provisioning
{: #json_eo}

```json
{
  "name": "hpvs-env-test",
  "crn": "crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:0b2df6e9-ec2c-4b4a-87dd-60f53f6a2a0d::",
  "region_id": "dal13",
  "tags": [],
  "state": "active",
  "status": "running",
  "resource_plan_title": "Free",
  "resource_plan_id": "bb0005a1-ec13-4ee4-86f4-0c3b15a357d5",
  "public_ip": "52.116.29.73",
  "internal_ip": "172.17.151.218",
  "storage": 53687091200,
  "memory": 2147483648,
  "vcpu": 1,
  "image_type": "self-provided",
  "image_os": "self-defined",
  "image_name": "de.icr.io/hpvs_test/ubuntu:v2",
  "free_time_left": 2156814134,
  "environment": {
    "TEST_VAR": "value",
    "TEST_VAR2": "value2"
  },
  "last_operation": "update succeeded"
}
```
{: codeblock}


## hpvs instance-create
{: #create_instance}

This command creates a new {{site.data.keyword.hpvs}} instance.


```sh
ibmcloud hpvs instance-create NAME PLAN LOCATION [--hostname HOST-NAME] [(--ssh SSH-KEY | --ssh-path SSH-KEY-PATH)] [(--rd REGISTRATION-DEFINITION | --rd-path REGISTRATION-DEFINITION-PATH)] [-i IMAGE-TAG] [-e ENV-CONFIG1 -e ENV-CONFIG2 ...] [-g RESOURCE-GROUP-ID] [-t TAG1 -t TAG2 ...] [--outbound-only]
```
{: codeblock}


`NAME`
:   The name of your new instance.

`PLAN`
:   The pricing plan ID or plan name for your Hyper Protect Virtual Server instance, for example, the plan name `lite-s` or its corresponding plan ID `bb0005a1-ec13-4ee4-86f4-0c3b15a357d5`. To list all plans and IDs, run the `ibmcloud catalog service hpvs` command.

`LOCATION`
:   The location ID to deploy your Hyper Protect Virtual Server instance to. To list available locations, run the `ibmcloud catalog service hpvs` command.


### Command options
{: #create_co}

`--hostname HOST-NAME`
:   The hostname that will be set within the {{site.data.keyword.hpvs}} container by using this parameter value.  

`--ssh SSH-KEY`
:   Public half of the SSH key to access the virtual server later. `--ssh` or `--ssh-path` is required when you use an IBM-provided image.

`--ssh-path SSH-KEY-PATH`
:   File path to the file that contains the public half of the SSH key to access the virtual server later. `--ssh` or `--ssh-path` is required when you use an IBM-provided image.

`--rd REGISTRATION-DEFINITION`
:   The encrypted and signed registration definition that is used for [Bring your own server image (BYOI)](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi). `--rd` or `--rd-path` is required when you use a self-provided image.

`--rd-path REGISTRATION-DEFINITION-PATH`
:   File path to the file that contains the encrypted and signed registration definition that is used for BYOI. `--rd` or `--rd-path` is required when you use a self-provided image.

`-i IMAGE-TAG`
:   The image tag for the BYOI server image. Required if you're using your own image.

`-t TAG`
:   Use tags to organize your resources. Tags are visible account-wide. Optional. Multiple tags are permitted, for example, `-t tag1 -t tag2`.

`-g RESOURCE-GROUP-ID | RESOURCE-GROUP-NAME`
:   The resource group to which your {{site.data.keyword.hpvs}} instance belongs for access control and billing purposes, for example, `Default`. To list all of your resource groups, run `ibmcloud resource groups`. Optional.

`-e ENV-CONFIG`
:   Specify environment variables if you are using a self-provided image. You must specify the variables in your registration definition first. You can set one or more environment variables as key value pairs by using the `-e` flag, for example, `-ibmcloud hpvs instance-update CRN -i latest -e k1=v1 -e k2='v2 v3'`. Environment variable `names` can have a maximum length of 64 characters and can be numbers, chars, and underscore. Environment variable `values` can have a maximum length of 12288.

`--outbound-only`
:   If this parameter is set, only outbound connections are allowed from your Hyper Protect Virtual Server instance. Use the internal IP address to connect to this Virtual Server from other Virtual Servers created by the same {{site.data.keyword.cloud_notm}} account in the same region.


### Example input
{: #create_ei}

```sh
ibmcloud hpvs instance-create MyHPVS lite-s dal13 -g Default --ssh "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCgguQtzV39LpP/iHAtjwo+4Z5QdASG73dwBlFIsTn5kPOaVYFHhzhvA/xMbLqDpxfYP/YzwU4rXNXMhCr4hlsruPXt5Ak4y83GmnNL8e+oq8lxU/afymje4PcYLnkm8WQvkreIEBaB73VOUKiLSSbdVljUk6a1LB347bCf72Oob8JpY4Pb3N4idrigSoCc+V4JVkz4pXD2Hoyar4J5I2527Ho+vUqdf5FoK9mFRUqtI8NTLKynL2/qVsCgTeUxnOknDjPE0+nqwyNI4toYozcISYb63K9Je6UBT4JaIQXMbdMhDH00wVH7R26SamKqS2iazcUBnZgN4//Vnic+US90ybsqvTuP/OQpHXwfdjshOEsz5PULZKbWgidsfA7aW3pjv1uijCPIrTFOsaAPktMCzhfJzaeFC0VIXweN7/2PT/Zl7U9Ys36CmmLaXfLotXxPWmbGUyRfavPN1Znhqph7v9w94E7JcngQ7sn+l+nkpYg5qdcBFZZ3kNhT4PVRbXE="
```
{: codeblock}

### Example output
{: #create_eo}

```sh
OK
Provisioning request for service instance 'crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:98338d1f-910b-4895-9410-453a9c4d9709::' was accepted.
 To check the provisioning status run:
 ibmcloud hpvs instance crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:98338d1f-910b-4895-9410-453a9c4d9709::
 ```
 {: codeblock}

## hpvs instance-update
{: #hpvsinstanceupdate}

This command updates a Hyper Protect virtual server instance.

```sh
ibmcloud hpvs instance-update (NAME | CRN) [--hostname HOST-NAME] [(--rd REGISTRATION-DEFINITION | --rd-path REGISTRATION-DEFINITION-PATH)] [-i IMAGE-TAG] [-e ENV-CONFIG1 -e ENV-CONFIG2 ...] [--force]
```
{: pre}


`NAME`
:   The name of your new instance. An error message is displayed if you specify a NAME that is not unique, for example,

```sh
VS name 'ABC' is ambiguous: more than one VS exist for the provided name: [crn1 crn2].
```
{: codeblock}

Specify the `CRN` if your instance name is not unique.

`CRN`
:   The server's Cloud resource name (CRN). Specify the `CRN` if `NAME` is not unique. You can run the `ibmcloud hpvs instances` command to get the CRN.


### Command options
{: #details_iu}

`--hostname HOST-NAME`
:   The hostname that will be set within the {{site.data.keyword.hpvs}} container by using this parameter value.  

`--rd REGISTRATION-DEFINITION`
:   The encrypted and signed registration definition that is used for [Bring your own server image (BYOI)](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi). `--rd` or `--rd-path` is optional when you use a self-provided image.

`--rd-path REGISTRATION-DEFINITION-PATH`
:   File path to the file that contains the encrypted and signed registration definition that is used for BYOI. `--rd` or `--rd-path` is optional when you use a self-provided image.

`-i IMAGE-TAG`
:   The image tag for the BYOI server image. Required if you're using your own image.

`-e ENV-CONFIG`
:   Specify environment variables when you use a self-provided image. They need to be specified in your registration definition first. You can set one or more environment variables as key value pairs by using the `-e` flag, for example, `-ibmcloud hpvs instance-update CRN -i latest -e k1=v1 -e k2='v2 v3'`. Environment variable `names` can have a maximum length of 64 characters and can be numbers, chars, and underscore. Environment variable `values` can have a maximum length of 4096.

`--force`
:   Forces the update of the {{site.data.keyword.hpvs}} instance without prompting for confirmation.


To check the status of the update, run `ibmcloud hpvs instance CRN` and check the value of `last operation` in the output.
{: note}

## hpvs instance-delete
{: #hpvsinstance_delete}

This command deletes a {{site.data.keyword.hpvs}}.

```sh
ibmcloud hpvs instance-delete (NAME | CRN)
```
{: codeblock}

### Command options
{: #delete_co}


`NAME`
:   The name of your new instance. An error message is displayed if you specify a NAME that is not unique, for example,

```sh
VS name 'ABC' is ambiguous: more than one VS exist for the provided name: [crn1 crn2].
```
{: codeblock}

Specify the `CRN` if your instance name is not unique.

`CRN`
:   The server's Cloud resource name (CRN). Specify the `CRN` if `NAME` is not unique. You can run the `ibmcloud hpvs instances` command to get the CRN.

`--force`
:   Forces the deletion of the {{site.data.keyword.hpvs}} instance without prompting for confirmation.


### Example output
{: #delete_eo}

```sh
Are you sure you want to delete the service instance? [y/N]> y

Deleting service instance crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:231fbbf4-1415-4162-86f0-9d50c260106d:: ...

OK
Service instance 'crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:231fbbf4-1415-4162-86f0-9d50c260106d::' was deleted.
You can restore a resource within 7 days after you delete it.
You can run the 'ibmcloud resource reclamations' command to check the resources that you can restore.
To irrecoverable delete the instance run the 'ibmcloud resource reclamation-delete RECLAMATION_ID' command.
```
{: codeblock}


When you delete a virtual server from the resource list, the server isn't deleted immediately, it stops and is marked for deletion. It is deleted after a reclamation period of seven days. During this seven-day reclamation period, you can restore the virtual server or manually trigger a deletion thru [resource reclamations](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}.
{: note}


## hpvs registration-key-create
{: #hpvsregistrationkeycreate}

This command creates a Hyper Protect virtual server gpg registration key. The resulting output files are required as inputs for the `hpvs registration-create` command, where it is used to sign the registration definition file for the deployment that uses your own image (BYOI). You need to run this command only when you do not have the gpg registration key or you have not already created the key pair, and when you want to use your own image (BYOI).

```sh
ibmcloud hpvs registration-key-create ID [--gpg-passphrase-path FILE-PATH] [-v VERBOSE]
```
{: pre}


`ID`
:   The user ID to set for the gpg registration key.


### Command options
{: #reggpgdetails}


`--gpg-passphrase-path FILE-PATH`
:   Is the path for the file that contains the passphrase that is being used for the registration key. The passphrase must consist of at least 6 characters. To make sure that a new line is not appended, use `echo` with `-n` or cat with EOF. If the path is not specified, you are prompted for the passphrase.

`-v, --verbose`
:   Set to true for verbose output.


### Example output
{: #create_regkey}

```sh
$ ibmcloud hpvs registration-key-create abcdefg
Enter password>
Successfully created registration key batch file 'abcdefg_registration-batch.txt' in the current working directory.

Successfully generated registration key with gpg.

Successfully exported the public part of the registration key to 'abcdefg.public' in the current working directory.

Successfully exported the private part of the registration key to 'abcdefg.private' in the current working directory.

To use this registration key to generate a registration definition file run `ibmcloud hpvs registration-create --registration-key-public-path abcdefg.public --registration-key-private-path abcdefg.private`
```
{: codeblock}

## hpvs registration-create
{: #hpvsregistrationfilecreate}

This command creates a registration definition file that is required to instantiate a Hyper Protect Virtual Server based on an own image. You need to run this command only when you want to use your own image (BYOI).

```sh
ibmcloud hpvs registration-create [--repository-name REPO-NAME] [--cr-username USER-NAME --cr-pwd-path FILE-PATH | --no-auth] [--allowed-env-keys ENV-KEYS | --no-env] [--image-key-id IMAGE-KEY-ID] [--fingerprint FINGERPRINT] [--image-key-public-path PUBLIC-KEY] [--registration-key-private-path PRIVATE-KEY-PATH] [--registration-key-public-path PUBLIC-KEY-PATH] [--gpg-passphrase-path PASS-PHRASE] [--cap-add CAPABILITIES] [--isv-secrets ISV-SECRETS | --no-isv-secrets]
```
{: pre}

If you enter the command without any parameters:

```sh
ibmcloud hpvs registration-create
```
{: pre}

You are prompted to enter all the parameters.

If image is in ICR, the fingerprint of the gpg key that was used to sign the image must be provided when you are prompted for the "Fingerprint", and path to the public key with which it is signed needs to be provided when you are prompted for the "Path to the file containing the image public key".

If the container registry does not require authentication, set the `-no-auth` parameter to prevent prompting. If no environment parameters are required, set the `-no-env` parameter, for example:

```sh
ibmcloud hpvs registration-create --no-env --no-auth
```
{: pre}

### Command options
{: #regfiledetails}


`--repository-name REPO-NAME`
:   Is the fully qualified name for the repository.

`--cr-username USER-NAME`
:   Is the username for the login on the container repository. It can be any string of 4 - 30 characters.

`--cr-pwd-path FILE-PATH`
:   Is the path to the file that contains the container repository password.

To ensure there are no trailing spaces in the file path, you can specify it as `vi -b file_name , :set noeol, :wq`.
{: note}

`--no-auth`
:   Is the parameter that must be set if the image does not require authorization to download. In this case, you don't need to provide `cr-username` and `cr-pwd-path` parameters. If you do, these parameters are ignored.

`--allowed-env-keys ENV-KEYS`
:   Specifies the allowed environment variable keys as a comma-separated list. The keys must be strings of 1 - 64 characters.

`--no-env`
:   This parameter can be set if the image does not require any allowed environment variables. In this case, you don't need to provide the `allowed-env-keys` parameter. If you do, it is ignored.

`--image-key-id IMAGE-KEY-ID`
:   The ID of the root key that was used to sign the image. It must contain 64 characters long. If the image-key-id is not specified, the command first tries to determine the ID automatically by requesting the container registry notary service. You are prompted for this parameter for Docker Hub images.

`--fingerprint FINGERPRINT`
:   If the image is in ICR, the fingerprint of the gpg key that is used to sign the image must be provided. You can get the fingerprint by using the `gpg --list-keys name_of_the_key` command. For example, when you run the `gpg --list-keys latestnewkey` command, the following snippet is an example of the output:
    ```buildoutcfg
    pub   rsa4096 2023-01-12 [SCEA] [expires: 2024-04-28]
          322A65D5B50BF16F5FDB6D7173943217FCD72F51
    uid           [ultimate] latestnewkey
    <latestnewkey@example.com>
    sub   rsa4096 2023-01-1--fingerprint FINGERPRINT2 [SEA] [expires: 2024-04-28]
    ```
    {: screen}

    In this example, "322A65D5B50BF16F5FDB6D7173943217FCD72F51" is the  fingerprint.

`--image-key-public-path PUBLIC-KEY`
:   The path for the file that contains the public part of the key that was used to sign the image. The public part of the key must be a minimum of 20 characters and base64 encoded. If the path is not specified, the command first tries to determine the public part of the key automatically by requesting the container registry notary service. If the image is present in ICR, path to the gpg public key that is used to sign the image should be mandatorily provided. It is optional for DCT signed image in Docker Hub.

`--registration-key-public-path PRIVATE-KEY-PATH`
:   The path for the public key from the registration key pair.

`--registration-key-private-path PUBLIC-KEY-PATH`
:   The path for the private key from the registration key pair.

`--gpg-passphrase-path PASS-PHRASE`
:   The path to a file that contains the `gpg` passphrase used for the private part of the registration key. The passphrase must consist of at least 6 characters. To make sure that a new line is not appended, use `echo` with `-n` or `cat` with EOF.

`--cap-add CAPABILITIES`
:   The Linux capabilities to be enabled are specified as a comma separated list.

`--isv-secrets ISV-SECRETS`
:   The Linux secrets to be used in BYOI. The secrets are added in the `/isv_secrets/secrets.json` file, within the container.

The ISV secrets is a key value pair that is separated by a colon, and you can specify a list secrets that are separated by a comma. Avoid adding spaces after the comma when you are specifying multiple secrets. If the size of the secret is large, it is recommended that you specify it by using the `--isv-secrets` command parameter. To update ISV secrets, you must create a new registration definition file with the updated ISV secrets and use the same registration definition file when running the `hpvs instance-update` command to update the instance.
{: note}

`--no-isv-secrets`
:   If the image does not require any isv secrets, this parameter can be set. You need not provide the isv-secrets parameter, and if you do, it will be ignored.


### Example output
{: #create_regfile}

In this sample, the registration-key-create command is run with some, but not all, parameters. The output (the last line in sample) shows the command with all parameters.
You are prompted for the password and the gpg pass phrase. You must use the path to a file that contains the password for the `--cr-pwd-path parameter` and the path to a file that contains the pass phrase for the `--gpg-passphrase-path` parameter. The sample command contains placeholders for those paths.

```sh
$ ibmcloud hpvs registration-create --registration-key-public-path abcdefg.public --registration-key-private-path abcdefg.private --isv-secrets key1:value1,key2:value2
Repository name> docker.io/containers/your-image
Container registry user name> username
Container registry password>
Allowed environment variables as comma separated list> ENV_PARAM
Allowed Linux capabilities to be enabled as comma separated list> AUDIT_CONTROL,AUDIT_WRITE
Gpg pass phrase for the private part of the registration key>
OK
The registration file was successfully created in the current working directory: registration.json.asc
Complete command executed:
 ibmcloud hpvs registration-create --repository-name=docker.io/containers/your-image --cr-username=username --cr-pwd-path=<PATH> --allowed-env-keys="ENV_PARAM" --registration-key-public-path=abcdefg.public --cap-add=AUDIT_CONTROL,AUDIT_WRITE --registration-key-private-path=abcdefg.private --gpg-passphrase-path=<PATH>  
 ```
{: codeblock}
