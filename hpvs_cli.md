---

copyright:
  years: 2020
lastupdated: "2020-07-15"

keywords: commands, cluster resource, hpvs-cli plugin, hpvs CLI, hpvs-cli command line , hpvs-cli shell


subcollection: hpvs-cli-plugin

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
{:shortdesc}

The {{site.data.keyword.hpvs}} CLI is available for the amd64 architecture on Linux, Windows, and Mac.

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

{{site.data.keyword.cloud_notm}} CLI requires Java&trade 1.8.0. You can download the CLI from {{site.data.keyword.cloud_notm}} to use on your local system as a complement to the {{site.data.keyword.cloud_notm}} console.
{: note}

## CLI commands
{: #plugin_use}

## hpvs help
{: #display_list}

This command displays a list of hpvs commands.

```
ibmcloud hpvs help
```
{: pre}


## hpvs instances
{: #instances_list}

This command lists all your {{site.data.keyword.hpvs}} instances.

```
ibmcloud hpvs instances [--output json]
```
{: pre}

### Command options

<dl>
<dt>`--output`</dt>
<dd>Displays results in the requested format. The only valid value is `json`.</dd>
</dl>


## hpvs instance
{: #details_list}

This command shows details about the server instance.

```
ibmcloud hpvs instance CRN [--output json]
```
{: pre}


<dl>
<dt>`CRN`</dt>
<dd>The server's Cloud resource name (CRN). You can run the `ibmcloud hpvs instances` command to get the CRN.</dd>
</dl>

### Command options

<dl>
<dt>`--output`</dt>
<dd>Displays results in the requested format. The only valid value is `json`.</dd>
</dl>

### Example output

```
Name                     hpvs-VS-test
CRN                      crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:231fbbf4-1415-4162-86f0-9d50c260106d::
Location                 dal13
Cloud State              active
Server Status            running
Plan                     Free
Public IP address        52.116.29.103
Internal IP address      172.17.151.218
Storage                  50 GiB
Memory                   2048 MiB
Processors               1 vCPUs
Image Type               ibm-provided
Image OS                 ubuntu18.04
Public Key Fingerprint   TSB2vIhKQ6czqHeIh05IxChkUUnEy4yKnU5Tlpl633U
```
### Example JSON output during provisioning
```
{
  "name": "hpvs-VS-test",
  "crn": "crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:231fbbf4-1415-4162-86f0-9d50c260106d::",
  "region_id": "dal13",
  "state": "active",
  "status": "running",
  "resource_plan_title": "Free",
  "resource_plan_id": "bb0005a1-ec13-4ee4-86f4-0c3b15a357d5",
  "public_ip": "52.116.29.103",
  "internal_ip": "172.17.151.218",
  "storage": 53687091200,
  "memory": 2147483648,
  "vcpu": 1,
  "image_type": "ibm-provided",
  "image_os": "ubuntu18.04",
  "public_key_fingerprint": "TSB2vIhKQ6czqHeIh05IxChkUUnEy4yKnU5Tlpl633U",
  "free_time_left": 2229325020
}
```

## hpvs instance-create
{: #create_instance}

This command creates a new {{site.data.keyword.hpvs}} instance.

```
ibmcloud hpvs instance-create NAME PLAN LOCATION [(--ssh SSH-KEY | --ssh-path SSH-KEY-PATH)] [(--rd REGISTRATION-DEFINITION | --rd-path REGISTRATION-DEFINITION-PATH)] [-i IMAGE-TAG] [-g RESOURCE-GROUP-ID] [-t TAG]
```
{: codeblock}


<dl>
<dt>`NAME`</dt>
<dd>The name of your new instance.</dd>
<dt>`PLAN`</dt>
<dd>The name or ID of your service plan, for example, the plan name for a free plan is `lite-s`. The possible values for a plan name are: `lite-s`, `entry`, `small` and `medium`.</dd>
<dt>`LOCATION`</dt>
<dd>The target location to create the service instance. The possible values are: `dal10`, `dal12`, `dal13`, `fra02`, `fra04`, `fra05`, `syd01`, `syd04`, `syd05`, `wdc04`, `wdc06`, `wdc07`.</dd>
</dl>

### Command options
<dl>
<dt>`--ssh SSH-KEY` </dt>
<dd>Public half of the SSH key to access the virtual server later. `--ssh` or `--ssh-path` is required when you use an IBM-provided image.</dd>
<dt>`--ssh-path SSH-KEY-PATH` </dt>
<dd>File path to the file that contains the public half of the SSH key to access the virtual server later. `--ssh` or `--ssh-path` is required when you use an IBM-provided image.</dd>
<dt>`--rd REGISTRATION-DEFINITION`</dt>
<dd>The encrypted and signed registration definition that is used for [Bring your own server image (BYOI)](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-byoi). `--rd` or `--rd-path` is required when you use a self-provided image.</dd>
<dt>`--rd-path REGISTRATION-DEFINITION-PATH`</dt>
<dd>File path to the file that contains the encrypted and signed registration definition that is used for BYOI. `--rd` or `--rd-path` is required when you use a self-provided image.</dd>
<dt>`-i IMAGE-TAG`</dt>
<dd>The image tag for the BYOI server image. Required if you're using your own image.</dd>
<dt>`-g RESOURCE-GROUP-ID | RESOURCE-GROUP-NAME` </dt>
<dd>The resource group to which your {{site.data.keyword.hpvs}} instance belongs for access control and billing purposes, for example, `Default`. To list all of your resource groups, run `ibmcloud resource groups`. Optional.</dd>
<dt>`-t TAG` </dt>
<dd>Use tags to organize your resources. Tags are visible account-wide. Optional. Multiple tags are permitted, for example, `-t tag1 -t tag2`.</dd>
</dl>

### Example input

```
ibmcloud hpvs instance-create MyHPVS lite-s dal13 -g Default -p "{\"sshPublicKey\": \"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCgguQtzV39LpP/iHAtjwo+4Z5QdASG73dwBlFIsTn5kPOaVYFHhzhvA/xMbLqDpxfYP/YzwU4rXNXMhCr4hlsruPXt5Ak4y83GmnNL8e+oq8lxU/afymje4PcYLnkm8WQvkreIEBaB73VOUKiLSSbdVljUk6a1LB347bCf72Oob8JpY4Pb3N4idrigSoCc+V4JVkz4pXD2Hoyar4J5I2527Ho+vUqdf5FoK9mFRUqtI8NTLKynL2/qVsCgTeUxnOknDjPE0+nqwyNI4toYozcISYb63K9Je6UBT4JaIQXMbdMhDH00wVH7R26SamKqS2iazcUBnZgN4//Vnic+US90ybsqvTuP/OQpHXwfdjshOEsz5PULZKbWgidsfA7aW3pjv1uijCPIrTFOsaAPktMCzhfJzaeFC0VIXweN7/2PT/Zl7U9Ys36CmmLaXfLotXxPWmbGUyRfavPN1Znhqph7v9w94E7JcngQ7sn+l+nkpYg5qdcBFZZ3kNhT4PVRbXE=\"}"
```

### Example output

```
OK
Provisioning request for service instance 'crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:98338d1f-910b-4895-9410-453a9c4d9709::' was accepted.
 To check the provisioning status run:
 ibmcloud hpvs instance crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:98338d1f-910b-4895-9410-453a9c4d9709::
 ```



## hpvs instance-delete
{: #hpvsinstance_delete}

This command deletes a {{site.data.keyword.hpvs}}.

```
ibmcloud hpvs instance-delete CRN
```
{: codeblock}

### Command options

<dl>
<dt>`CRN`</dt>
<dd>The server's Cloud resource name (CRN). You can run the `ibmcloud hpvs instances` command to get the CRN.</dd>
<dt>--force</dt>
<dd>Forces the deletion of the {{site.data.keyword.hpvs}} instance without showing a confirmation prompt.</dd>
</dl>

### Example output

```
Are you sure you want to delete the service instance? [y/N]> y

Deleting service instance crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:231fbbf4-1415-4162-86f0-9d50c260106d:: ...

OK
Service instance 'crn:v1:staging:public:hpvs:dal13:a/1075962b93044362a562c8deebbfba2e:231fbbf4-1415-4162-86f0-9d50c260106d::' was deleted.
You can restore a resource within 7 days after you delete it.
You can run the 'ibmcloud resource reclamations' command to check the resources that you can restore.
To irrecoverable delete the instance run the 'ibmcloud resource reclamation-delete RECLAMATION_ID' command.
```


{:note}
When you delete a virtual server from the resource list, the server isn't deleted immediately, it's stopped and marked for deletion. It's deleted after a reclamation period of seven days. During this seven-day reclamation period, you can restore the virtual server or manually trigger a deletion via [resource reclamations](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}.
