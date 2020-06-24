---

copyright:
  years: 2020
lastupdated: "2020-06-23"

subcollection: hp-virtual-servers

keywords: CLI, command line interface, cli

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Using the {{site.data.keyword.cloud_notm}} CLI with {{site.data.keyword.hpvs}}
{: #clihpvs}

You can use the {{site.data.keyword.cloud}} CLI to list, create, or delete your {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} instances.
{:shortdesc}

## Prerequisites
{: #prerequisitecli}
- The {{site.data.keyword.cloud_notm}} CLI must be installed and configured, and you must be logged in via the ibmcloud login.
- No additional plug-ins are required.

{:note}
Not all of the data is available through the CLI. For example, you still need to log into the {{site.data.keyword.cloud_notm}} UI to get the IP addresses for a {{site.data.keyword.hpvs}} instance.

## Setting up the {{site.data.keyword.cloud_notm}} CLI
{: #ibmcloudcli}

Follow these [instructions](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started){: external} to install and configure the {{site.data.keyword.cloud_notm}} CLI.

## Listing instances with the {{site.data.keyword.cloud_notm}} CLI
{: #clilist}

To list all of your service instances in your resource list including {{site.data.keyword.hpvs}}, enter:

```
ibmcloud resource service-instances
```
{: codeblock}

Example output:

```
Name   Location State  Type
MyHPVS dal13    active service_instance
```
{: codeblock}

If you have no instances, a no instance found message is displayed.
You can find more information about the list command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances){: external}.

## Listing resource groups with the {{site.data.keyword.cloud_notm}} CLI
{: #cliresource}

To list all of your [resource groups](https://cloud.ibm.com/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups), enter:

```
ibmcloud resource groups
```
{: codeblock}

Example output:

```
Name      ID                                 Default Group   State   
Default   c4c083abbf134886883546ac03bddb5b   true            ACTIVE
```
{: codeblock}

You can find more information about the resource groups command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_groups){: external}


## Creating an {{site.data.keyword.hpvs}} instance with the {{site.data.keyword.cloud_notm}} CLI
{: #clicreate}

Before you create a new {{site.data.keyword.hpvs}} instance, make sure you have the following information available for your new instance:

- NAME: A name of your new instance.
- SERVICE_NAME or SERVICE_ID: The name of the service is `hpvs` and the ID is `986f2197-9f9a-44f4-9463-f17ec64c6729`.
- SERVICE_PLAN_NAME or SERVICE_PLAN_ID: The service plan name or ID, for example, the plan name for a free plan is `lite-s`. To list the plans use the `ibmcloud catalog service hpvs` [command](https://cloud.ibm.com/docs/resources?topic=resources-changing#changing_command_line){: external}.
- LOCATION: The target location to create the service instance, for example, `dal13`. You can list all plans and the available locations for the plans via `ibmcloud catalog service hpvs` [command](https://cloud.ibm.com/docs/resources?topic=resources-changing#changing_command_line){: external}.
- Resource group:  The [resource group](https://cloud.ibm.com/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups) to which your {{site.data.keyword.hpvs}} instance belongs for access control and billing purposes, for example, `default`.
- Parameters @JSONFILE or JSON_STRING: The JSON file or JSON string of parameters to create service instance. To create a new {{site.data.keyword.hpvs}} instance you need to provide your public SSH key. The parameter name is `sshPublicKey`.

Use the `ibmcloud service-instance-create` command and specify the required options:

```
ibmcloud resource service-instance-create NAME (SERVICE_NAME | SERVICE_ID) SERVICE_PLAN_NAME LOCATION -g, --resource group -p, --parameters (@JSON_FILE | JSON_STRING )
```
{: codeblock}


For example:
```
ibmcloud resource service-instance-create MyHPVS hpvs lite-s dal13 -g Default -p "{\"sshPublicKey\": \"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCgguQtzV39LpP/iHAtjwo+4Z5QdASG73dwBlFIsTn5kPOaVYFHhzhvA/xMbLqDpxfYP/YzwU4rXNXMhCr4hlsruPXt5Ak4y83GmnNL8e+oq8lxU/afymje4PcYLnkm8WQvkreIEBaB73VOUKiLSSbdVljUk6a1LB347bCf72Oob8JpY4Pb3N4idrigSoCc+V4JVkz4pXD2Hoyar4J5I2527Ho+vUqdf5FoK9mFRUqtI8NTLKynL2/qVsCgTeUxnOknDjPE0+nqwyNI4toYozcISYb63K9Je6UBT4JaIQXMbdMhDH00wVH7R26SamKqS2iazcUBnZgN4//Vnic+US90ybsqvTuP/OQpHXwfdjshOEsz5PULZKbWgidsfA7aW3pjv1uijCPIrTFOsaAPktMCzhfJzaeFC0VIXweN7/2PT/Zl7U9Ys36CmmLaXfLotXxPWmbGUyRfavPN1Znhqph7v9w94E7JcngQ7sn+l+nkpYg5qdcBFZZ3kNhT4PVRbXE=\"}"

```
{: codeblock}

You can find more information about the create command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_create){: external}.

If the service instance is created successfully, a message is displayed informing you together with some other details, such as ID, current state and so on.

The newly created instance is marked as provisioning until provisioning has completed. Use the `ibmcloud resource service-instances` command to list your new instance and check its current state. After provisioning has completed, the instance is marked as active.

{:note}
You can't retrieve the IP addresses of the newly created instance via the {{site.data.keyword.cloud_notm}} CLI. You need to log in to the {{site.data.keyword.cloud_notm}} UI and navigate to the resource details page and get the IP addresses as described [here](https://cloud.ibm.com/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs).


## Deleting an {{site.data.keyword.hpvs}} instance with {{site.data.keyword.cloud_notm}} CLI
{: #clidelete}

You can delete an {{site.data.keyword.hpvs}} instance via the `ibmcloud resources service-instance-delete (NAME | ID) command`.

where:
- Name or ID: The name or ID of the service instance. You can get the name via `ibmcloud resource service-instances` and the ID via `ibmcloud resources service-instances --long`.

For example:
```
ibmcloud resources service-instance-delete MyHPVS
```
{: codeblock}

You can find more information about the delete command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_delete){: external}.

Enter `y` to confirm that you want to delete the instance. The {{site.data.keyword.cloud_notm}} CLI notifies you that the instance was successfully deleted.

{:note}
When you delete a virtual server from the resource list, the server is not deleted immediately, it is stopped and marked for deletion after a reclamation period of seven days. During this seven day reclamation period you can restore the virtual server or manually trigger a deletion via [resource reclamations](https://cloud.ibm.com/docs/resources?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}.
