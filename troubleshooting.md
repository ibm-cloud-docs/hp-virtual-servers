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
