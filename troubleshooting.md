---

copyright:
  years: 2019
lastupdated: "2019-08-16"

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

If you run into problems while you are using {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}}, you can recover from these problems in many cases by following a few easy steps.
{:shortdesc}

## Why can't I see system details on the dashboard of my created server instance?
{: #no_details_hpvs}

Though a new virtual server instance has been successfully created, no system information is available.
{: troubleshoot}

The dashboard of your new virtual server instance does not show configuration details of your system. Values for system characteristics, like, for example, CPU, memory, or storage details are not available.
{: tsSymptoms}

A delay of configuration information display is due to authentication.
{: tsCauses}

To see the missing information, delete your browser cache, log out of the {{site.data.keyword.cloud_notm}}, and log in again.
{: tsResolve}

## Why can't I log in to my recently provisioned server instance?
{: #no_connection_hpvs}

I have just created an instance and trying to connect for the first time.
{: troubleshoot}

You cannot access your recently created virtual server.  Connection fails with the following message:

```
Connection refused
```
{: codeblock}
{: tsSymptoms}

Network configuration might take up to 30 minutes, even after the status switched to ´Provisioned´ in the **Resource list**.
{: tsCauses}

Wait about 30 minutes to allow the configuration completion and then retry to access your instance.
{: tsResolve}
