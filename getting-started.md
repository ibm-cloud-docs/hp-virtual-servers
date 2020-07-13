---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-09"

subcollection: hp-virtual-servers

keywords: hyper protect virtual servers, Hyper Protect Virtual Servers, getting started

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Getting started tutorial  
{: #getting-started}

Using the {{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} service, you can provision virtual servers on a highly secure stack with an Ubuntu Linux operating system. On such a virtual server instance, you can run a myriad of Linux applications and containerized workloads.

This introduction informs you about the prerequisites for using this service. It points you to the process for provisioning a virtual server and informs you how to retrieve information about a created virtual server.

Finally, you find a reference to a description how to log in to your new virtual server.
{:shortdesc}


## Prerequisites
{: #gs_prerequisites}

For an overview of the features and advantages of this service, read [{{site.data.keyword.hpvs}} overview](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-overview).

Before you start:
- Make sure you have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/docs/account?topic=account-signup){: external}.
- If you want to use a {{site.data.keyword.hpvs}} paid plan, you require a [Pay-As-You-Go account](https://cloud.ibm.com/docs/account?topic=account-upgrading-account){: external}. To check your account type, go to [{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/login){: external} and click **Management** > **Account** > **Account settings**.
- Check whether you are using the [required browser software](/docs/overview?topic=overview-prereqs-platform) for {{site.data.keyword.cloud_notm}}.
- Have an SSH key pair ready, which you need for creating and connecting to your virtual server instance. Read [Generating SSH keys](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-generate_ssh) for more information.

Watch the following video to find how to get started with Hyper Protect Virtual Server:

<iframe width="737" height="415" title="Getting Started with IBM Cloud Hyper Protect Virtual Servers" src="https://www.youtube.com/embed/GlP-w-vsPmc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Provisioning a {{site.data.keyword.hpvs}} instance
{: #gs_provisioning}

You create a virtual server instance in a few steps from the user interface on the {{site.data.keyword.hpvs}} page.
Read [Provisioning a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-provision) for detailed information.

## Retrieving virtual server information
{: #gs_retrieving}

After you created an instance of {{site.data.keyword.hpvs}}, you can check the detailed information of your virtual server.
Read [Retrieving information about a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs) for more information.

## Connecting to a new virtual server
{: #gs_logging}

From your client workstation, you can connect to a virtual server that exists on {{site.data.keyword.cloud_notm}} with its public IP address and your corresponding private SSH key. Read the information in [Connecting to a new virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-connect_vs).

## Managing user access
Access to the management of {{site.data.keyword.hpvs}} service instances is controlled by IBM Cloud Identity and Access Management (IAM) whereas the servers use standard Ubuntu user-access management. You can find more information  [here](https://cloud.ibm.com/docs/hp-virtual-servers?topic=hp-virtual-servers-manage_access){: external}.
