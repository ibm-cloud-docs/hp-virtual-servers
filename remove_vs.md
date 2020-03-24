---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-19"

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

Go to the [Resource list](https://cloud.ibm.com/resources){: external} (see [Retrieving virtual server information](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs)) to delete a virtual server. Select the instance from the **Services** list and apply the **Delete** action from its pull-down choice. Alternatively, you can delete the server using the {{site.data.keyword.cloud_notm}} cli.
{:shortdesc}

![Deleting a virtual server instance](image/hpvs_delete_instance.gif "Deleting a virtual server instance")
*Figure 1. Deleting a virtual server instance*


When you delete a virtual server from the resource list, the server is not deleted immediately, it is stopped and marked for deletion after a reclamation period of seven days. During this seven day reclamation period you can restore the virtual server or manually trigger a deletion via [resource reclamations](https://cloud.ibm.com/docs/resources?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}. Resource reclamations lists the Hyper Protect virtual servers that are marked for deletion together with the time (Target time) when the actual deletion will be triggered.

## Deleting servers belonging to the free plan
The free plan servers expire 30 days after they have been created. When a server expires, it is immediately be deleted at the backend. This also applies if the server expires during the seven-day reclamation period. However, the server is still displayed in the resource list or in the resource reclamations as if it still existed even though it has already been deleted at the backend.
If the server has expired, you can remove the entry from the resource list and either wait for the target time to trigger the entry to be deleted or manually trigger the deletion from the resource reclamations. If an expired server is restored, only the entry within the resource list is restored, not the server itself. This is because the server has already been deleted.  

## Restoring servers belonging to paid plans
Servers belonging to any of the paid plans will not expire and therefore can be restored during the reclamation period. If the server is restored, the timeframe for which the server was within the resource reclamations will be billed to your account.

After the server has been deleted, only metadata which is not considered to be personal data is kept for 6 months.
Therefore, make sure you back up all important data for future use. There is no possibility to recover data after the virtual server has been deleted.
{:important}
