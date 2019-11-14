---

copyright:
  years: 2019
lastupdated: "2019-11-13"

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

## Status 'Provision failure' is displayed in the **Resource list** when you create multiple virtual servers
{: #provision_failure}

Creating more than five virtual server instances nearly simultaneously in one data center without any existing instances, results in the creation of one or more virtual servers with status 'Provisioning failure' within the **Resource list**.
{: troubleshoot}

You try to create more than five virtual servers within a short period in one data center without any existing virtual server instances.
{: tsSymptoms}

The number of virtual servers per data center is limited to five. Creating more than five virtual servers nearly at the same time in one data center leads to status 'Provisioning failure' for the sixth and all subsequent instances.
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
