---

copyright:
  years: 2019
lastupdated: "2019-10-25"

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

Though a new virtual server instance has been successfully created, the details of the system are displayed incorrectly or are not displayed at all.
{: troubleshoot}

The dashboard of your new virtual server instance does not show configuration details of your system, or these details are not presented in the correct manner.
{: tsSymptoms}

This issue may have multiple causes. Most probable, there is either a caching issue, or you might be using an unsupported browser.
{: tsCauses}

Verify that you are using a supported browser. Delete your browser cache, log out of the {{site.data.keyword.cloud_notm}}, and log in again.
{: tsResolve}

## Why can't I log in to my recently provisioned server instance?
{: #no_connection_hpvs}

I have just created an instance and now I am trying to connect for the first time.
{: troubleshoot}

You cannot access your recently created virtual server.  Connection fails with message:
`Connection refused`
{: tsSymptoms}

Network configuration might take up to 30 minutes, even after the status switched to ´Provisioned´ in the **Resource list**.
{: tsCauses}

Wait about 30 minutes to allow the configuration completion and then retry to access your instance.
{: tsResolve}

## Status 'Provision failure' is displayed in the **Resource list** when creating multiple instances
{: #provision_failure}

Creating multiple instances at once or within a very short period of time, in one availability zone which did not have any instances before, results in the creation of one more instances with status 'Provisioning failure' within the **Resource list**.
{: troubleshoot}

You either try to create more than five instances within a very short period of time. This can happen in an availability zone even without any existing instances, if do not need to create a new public SSH key, but reuse the same public key for all of the instances you want to create. Or you already have some instances ready, and you want to create a few more instances, so that the sum of all instances will exceed the maximum number of five.  
{: tsSymptoms}

The amount of instances per account and per availability zone is limited to five. Creating more than five instances in one zone will lead to a provisioning failure.
{: tsCauses}

{: tsResolve}
You can either provision more than five instances in different availability Zones. Or you can provision more than fice instances using different {{site.data.keyword.cloud_notm}} accounts.
