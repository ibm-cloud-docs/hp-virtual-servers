---

copyright:
  years: 2019
lastupdated: "2019-10-24"

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Getting started tutorial  <!-- ******** with {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} ****** -->
{: #getting-started}

{{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} is in the **Beta** phase and is for tryout and test purpose only. The virtual server that you create with {{site.data.keyword.hpvs}} will be deleted after 30 days. To prevent data loss, use only test data in the current service. This restriction also applies to using {{site.data.keyword.hpvs}} with other {{site.data.keyword.cloud_notm}} services.
{:important}

The {{site.data.keyword.hpvs}} service provides a virtual server on a highly secure stack with an Ubuntu Linux operating system. On such a server, you can run a myriad of Linux applications and Docker workloads.

This introduction informs you about the prerequisites for using this service. It points you to the process for provisioning a {{site.data.keyword.hpvs}} instance and informs you how to retrieve information about an instance of a created virtual server.
Finally, you find a reference to a description how to log in to your new virtual server.
{:shortdesc}

## Prerequisites
{: #prerequisites}

For an overview of the features and advantages of this service, read [{{site.data.keyword.hpvs}} overview](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-overview).

Before you start, check whether you are using the [required browser software](/docs/overview?topic=overview-prereqs-platform) for {{site.data.keyword.cloud_notm}}. Also, have an SSH key pair ready, which you need for creating and connecting to your server instance. Read [Generating SSH keys](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-generate_ssh) for more information.


## Provisioning a {{site.data.keyword.hpvs}} instance

You create a virtual server instance in a few steps from the user interface on the {{site.data.keyword.hpvs}} page.
Read [Provisioning a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-provision) for detailed information.
A virtual server instance is also called HP-VS instance for short.

## Retrieving virtual server information

After you created an instance of {{site.data.keyword.hpvs}}, you can check the detailed information of your virtual server.
Read [Retrieving information about a virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-retrieve-info-vs) for more information.


## Logging-in to a new virtual server

From your client workstation, you can connect to a virtual server that exists on {{site.data.keyword.cloud_notm}} with the public IP address of the server instance and your corresponding private SSH key. Read the information that is provided in [Logging-in to a new virtual server](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-connect_vs).
