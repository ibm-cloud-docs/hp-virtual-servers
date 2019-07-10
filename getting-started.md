---

copyright:
  years: 2019
lastupdated: "2019-07-10"

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Getting started with {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}}
{: #getting-started-with-hp-virtual-servers}

{{site.data.keyword.cloud}} {{site.data.keyword.hpvs}} is in the **Experimental** phase and is for tryout and test purpose only. <!-- **** next sentence was NO LONGER  in the staging version, but still in  the public version, so check, befor activating to public to re-delete it again.**** --> You need to sign-up via the [Hyper Protect info page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/hyper-protect-services){:new_window} and request to be added to the entitlement list before you can see this service in the Cloud Experimental catalog. <!-- **** End of re-activated sentence **** -->The virtual server that you create with {{site.data.keyword.hpvs}} will be deleted after 30 days. To prevent data loss, use only test data in the current service. This restriction also applies to using {{site.data.keyword.hpvs}} with other {{site.data.keyword.cloud_notm}} services.
{:important}

<!-- ******** {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} is an {{site.data.keyword.cloud_notm}} service that provides highly secure virtual servers that can run Linux and Docker workloads on demand. It offers a flexible and scalable platform that allows you to quickly and easily provision and manage the virtual server of your choice, which allows for a range of capacity sizes to meet various demands of applications that run in the server.-->

This {{site.data.keyword.cloud_notm}} offering instantiates a base open source Ubuntu Linux image inside each virtual server, which can run a myriad of Linux applications that include Docker. This tutorial guides you how to retrieve virtual server information using {{site.data.keyword.hpvs}}.
{:shortdesc}

## Prerequisite
{: #prerequisite}

Before you start, make sure you are using the [required browser software](/docs/overview?topic=overview-prereqs-platform) for {{site.data.keyword.cloud_notm}}.


## Provision a {{site.data.keyword.hpvs}} instance
For detailed steps, see [Provisioning {{site.data.keyword.hpvs}}](/docs/services/hp-virtual-servers?topic=hp-virtual-servers-provision).

## Retrieving virtual server information
{: #retrieve-vs-info}

After you created an instance of {{site.data.keyword.hpvs}}, you can check the detailed information of your virtual server.

1. Go to the virtual server instance on {{site.data.keyword.cloud_notm}}.
2. Click the **Manage** tab from the left navigation.
3. Click **OPEN** to open the {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} dashboard.

You can then find detailed information of your virtual server in the dashboard, including IP address, which you will use to connect to the virtual server.


## Next steps
{: #next-steps}

Now you have created your virtual server on {{site.data.keyword.cloud_notm}}. You can connect to your virtual server with the IP address of the service instance and your corresponding private key.
