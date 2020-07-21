---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-15"

subcollection: hp-virtual-servers

keywords: deleting virtual server, resource reclamations
---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Deleting a virtual server
{: #remove_vs}

You can use the {{site.data.keyword.hpvs}} UI or the CLI to delete virtual servers. Deleted servers that belong to paid plans can also be restored before the reclamation period expires.
{:shortdesc}

## Deleting a virtual server in the UI

1. Go to the [Resource list](https://cloud.ibm.com/resources){: external} (see [Retrieving virtual server information](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs)) to delete a virtual server.
2. Select the instance from the **Services** list and apply the **Delete** action from its pull-down choice.


![Deleting a virtual server instance](image/hpvs_delete_instance.gif "Deleting a virtual server instance")
*Figure 1. Deleting a virtual server instance*

## Deleting a virtual server from the CLI

To delete {{site.data.keyword.hpvs}} from the [CLI](https://cloud.ibm.com/docs/hpvs-cli-plugin):

1. Make sure you know the Cloud resource name (CRN) of the server you want to delete. To find this out, run:

```
ibmcloud hpvs instances
```
{:pre}

2. To delete the server, run the following command:

```
ibmcloud hpvs instance-delete CRN --force
```
{:pre}
where `CRN` is Cloud resource name of the server you want to delete. Use `--force` to force the deletion of the {{site.data.keyword.hpvs}} instance without showing a confirmation prompt


For more information and example output, see [here](https://test.cloud.ibm.com/docs/hpvs-cli-plugin#hpvs-instance-delete).

## What happens during the reclamation period
When you delete a virtual server from the resource list, the server is not deleted immediately, it is stopped and marked for deletion, and at this point a reclamation period of seven days starts. The server is deleted after the reclamation period ends. During this seven day reclamation period you can restore the virtual server or manually trigger a deletion via [resource reclamations](https://cloud.ibm.com/docs/resources?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}. Resource reclamations lists the Hyper Protect virtual servers that are marked for deletion together with the time (Target time) when the actual deletion will be triggered.

## Deleting servers belonging to the free plan
The free plan servers expire 30 days after they have been created. When a server expires, it is immediately be deleted at the backend. This also applies if the server expires during the seven-day reclamation period. However, the server is still displayed in the resource list or in the resource reclamations as if it still existed even though it has already been deleted at the backend.
If the server has expired, you can remove the entry from the resource list and either wait for the target time to trigger the entry to be deleted or manually trigger the deletion from the resource reclamations. If an expired server is restored, only the entry within the resource list is restored, not the server itself. This is because the server has already been deleted.  

## Restoring servers belonging to paid plans
Servers belonging to any of the paid plans will not expire and therefore can be restored during the reclamation period. If the server is restored, the timeframe for which the server was within the resource reclamations will be billed to your account.

After the server has been deleted, only metadata which is not considered to be personal data is kept for 6 months.
Therefore, make sure you back up all important data for future use. There is no possibility to recover data after the virtual server has been deleted.
{:important}
