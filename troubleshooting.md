---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-23"

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
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Solving problems
{: #troubleshooting}

If you run into problems while you are using {{site.data.keyword.hpvs}}, you can recover from these problems in many cases by following a few steps.
{:shortdesc}

## You cannot see the details of your virtual server instance on its dashboard - or the dashboard seems to display incorrectly
{: #no_details_hpvs}

Though a new virtual server was successfully created, the details of the system are displayed incorrectly or are not displayed at all.
{: troubleshoot}

The dashboard of your new virtual server does not show configuration details of your system, or these details are not presented in the correct manner.
{: tsSymptoms}

This issue can have multiple causes. Most probable, there is either a caching issue, or you might be using an unsupported browser.
{: tsCauses}

Verify that you are using a supported browser. Delete your browser cache, log out of the {{site.data.keyword.cloud_notm}}, and log in again.
{: tsResolve}

## Status 'Inactive' is displayed in the **Resource list** when you create multiple virtual servers
{: #provision_failure}

Creating more than five virtual server instances almost simultaneously in one data center without any existing instances, results in the creation of one or more virtual servers with the status 'Inactive' (provisioning failed) within the **Resource list**, and when you click the 'Inactive' server in **Name** field, an error message is displayed to indicate that provisioning has failed.
{: troubleshoot}

You try to create more than five virtual servers within a short period in one data center without any existing virtual server instances.
{: tsSymptoms}

The number of virtual servers per data center is limited to five. Creating more than five virtual servers almost all at the same time in one data center leads to the status 'Inactive' (provisioning failed) for the sixth and all subsequent instances.
{: tsCauses}

You can either provision more than five virtual servers in different data centers. Or you can provision more than five instances by using different {{site.data.keyword.cloud_notm}} accounts.
{: tsResolve}


##  Generating a new virtual server fails due to exhausted resources
{: #max_number_resources}

Each data center where you can create virtual server instances offers limited resources for a certain number of such instances.  
{: troubleshoot}

When you provision a new virtual server, you get an error message because the resources (for example, memory) are exhausted.
{: tsSymptoms}

The resources for the selected data center are exhausted.
{: tsCauses}

Retry the action and select a different data center.
{: tsResolve}


## Cannot connect to free virtual server anymore
{: #connect_freevs}

I cannot connect to a server (for example, via SSH) which is in the free plan although it is visible in the {{site.data.keyword.cloud_notm}} resource list.
{: troubleshoot}

You cannot connect to a server in the free plan anymore.
The server is still visible in the resource list, but the dashboard shows an error message. The message indicates that the server expired. This means that the server and all data that was stored on the server have been deleted.
{: tsSymptoms}

All virtual servers in free plans are deleted after 30 days.
{: tsCauses}

Virtual servers in paid plans are not deleted. You can remove the server from the resource list by deleting it from the resource list.
{: tsResolve}


## Cannot create new virtual servers due to exhausted resources or plan limitations
{: #connect_newvs}

You cannot create new virtual servers due to exhausted resources or plan limitations, although you have less than the described limits.
{: troubleshoot}

Whilst creating a new server you get either a message informing you that the maximum number of servers has been reached in the selected data center for this account, or that you have reached the maximum number of free plans for this account. When you look in the resource list, you see that you have not reached the limit.
{: tsSymptoms}

The resource list does not reflect the complete list of all the servers that you have. If you “delete” a server from the resource list, it is made inaccessible and marked for deletion, but it is still available for you within the [resource reclamations](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external} for a limited amount of time, as described in [Deleting a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-remove_vs).
{: tsCauses}

Depending on your requirements you should either delete the resource from the resource reclamations, or select a different data center (maximum number of servers has been reached), or select the appropriate paid plan that meets your requirements if you have reached the maximum number of free plans.
For more information, see [resource reclamations](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations){: external}.
{: tsResolve}
