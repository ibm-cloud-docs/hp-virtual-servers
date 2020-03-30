---

copyright:
  years: 202ß
lastupdated: "2020-03-26"

subcollection: hp-virtual-servers

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Using the IBM Cloud CLI with {{site.data.keyword.hpvs}}
{: #clihpvs}

You can use the {{site.data.keyword.cloud_notm}} CLI to list, create, or delete your {{site.data.keyword.hpvs}} instances.
{:shortdesc}

## Prerequisites
{: #prerequisitecli}
- The {{site.data.keyword.cloud_notm}} CLI must be installed and configured, and you must be logged in via the ibmcloud login.
- No additional plug-ins are required.

{:note}
Not all of the data is available through the CLI. For example, you still need to log into the {{site.data.keyword.cloud_notm}} UI to get the IP addresses for a {{site.data.keyword.hpvs}} instance.

## Setting up the {{site.data.keyword.cloud_notm}} CLI
{: #ibmcloudcli}

Follow these [instructions](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started) to install and configure the {{site.data.keyword.cloud_notm}} CLI.

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
You can find more information about the list command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cliibmcloud_commands_resource#ibmcloud_resource_service_instances).

## Creating an {{site.data.keyword.hpvs}} instance with I{{site.data.keyword.cloud_notm}} CLI
{: #clicreate}

To create a new {{site.data.keyword.hpvs}} instance, use the `ibmcloud service-instance-create` command and specify the required options:

```
ibmcloud resource service-instance-create NAME (SERVICE_NAME | SERVICE_ID) SERVICE_PLAN_NAME LOCATION -p, --parameters (@JSON_FILE | JSON_STRING )
```
{: codeblock}

where:
- NAME: The name your instance.
- SERVICE_NAME or SERVICE_ID: The name of the service is hpvs and the id is 986f2197-9f9a-44f4-9463-f17ec64c6729.
- SERVICE_PLAN_NAME or SERVICE_PLAN_ID: The service plan name or ID. To list the plans use the [ibmcloud catalog service hpvs command](https://cloud.ibm.com/docs/resources?topic=resources-changing#changing_command_line).
- LOCATION: The target location to create the service instance. You can list all plans and the available locations for the plans via [ibmcloud catalog service hpvs command](https://cloud.ibm.com/docs/resources?topic=resources-changing#changing_command_line).
- p, --parameters @JSONFILE or JSON_STRING: The JSON file or JSON string of parameters to create service instance. To create a new {{site.data.keyword.hpvs}} instance you need to provide your public SSH key. The parameter name is `sshPublicKey`.

For example:
```
ibmcloud resource service-instance-create MyHPVS hpvs lite-s dal13 -p '{"sshPublicKey": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQCu77Csf9kDmfaeiShCxwvL2/JUYhBZ2tP9KuvCU+z9xzewb230TfOlHSLYejEaJ/TMohnHHnlaX9fsvkRjVmpC0IUWELi6ywpNKkOmLfJ21QzNDfnP8YczliyQd1c230x5RlZ CDF6ZEOULbVmCijsrCbVlOw1OKGdWOu1U4fP5tw=="}
```
{: codeblock}

You can find more information about the create command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cliibmcloud_commands_resource#ibmcloud_resource_service_instance_create).

If the service instance is created successfully, a message is displayed informing you together with some other details, such as ID, current state and so on.

The newly created instance is marked as provisioning until provisioning has completed. Use the `ibmcloud resource service-instances` command to list your new instance and check its current state. After provisioning has completed, the instance is marked as active.

{:note}
You can't retrieve the IP addresses of the newly created instance via the {{site.data.keyword.cloud_notm}} CLI. You need to log in to the I{{site.data.keyword.cloud_notm}} UI and navigate to the resource details page and get the IP addresses as described [here](https://cloud.ibm.com/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieveinfo-vs).


## Deleting an {{site.data.keyword.hpvs}} instance with {{site.data.keyword.cloud_notm}} CLI
{: #clidelete}

You can delete an {{site.data.keyword.hpvs}} instance via the `ibmcloud resources service-instance-delete (NAME | ID) command`.

where:
- Name or ID: The name or ID of the service instance. You can get the name via `ibmcloud resource service-instances` and the ID via `ibmcloud resources service-instances --long`.

For example:
```
ibmcloud resource service-instance-delete MyHPVS
```
{: codeblock}

You can find more information about the delete command [here](https://cloud.ibm.com/docs/cli?topic=cloud-cliibmcloud_commands_resource#ibmcloud_resource_service_instance_delete).

Enter `y` to confirm that you want to delete the instance. The {{site.data.keyword.cloud_notm}} CLI notifies you that the instance was successfully deleted.

{:note}
When you delete a virtual server from the resource list, the server is not deleted immediately, it is stopped and marked for deletion after a reclamation period of seven days. During this seven day reclamation period you can restore the virtual server or manually trigger a deletion via [resource reclamations](https://cloud.ibm.com/docs/resources?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}.
